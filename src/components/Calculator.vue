<template>
  <div class="calculator-grid">
    <div class="calculator-header">
      <div class="calculator-str">{{ display || "0" }}</div>
      <div class="calculator-sum" ref="current">
        <span ref="currentValue">{{ currentValue | currency }}</span>
      </div>
    </div>
    <div class="calculator-body">
      <div class="calculator-row" v-for="row in buttons" :key="row.text">
        <div
          v-for="button in row"
          :key="button.text"
          :class="{
            'calculator-item': true,
            'btn-operator': button.type == 'operator',
            ' btn-large': button.style == 'btn-large',
            'btn-light': button.style == 'btn-light'
          }"
          @click="buttonClick(button)"
        >
          <div>{{ button.text }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
let buttons = [
  [
    {
      text: "7",
      type: "number"
    },
    {
      text: "8",
      type: "number"
    },
    {
      text: "9",
      type: "number"
    },
    {
      text: "÷",
      type: "operator"
    }
  ],
  [
    {
      text: "4",
      type: "number"
    },
    {
      text: "5",
      type: "number"
    },
    {
      text: "6",
      type: "number"
    },
    {
      text: "×",
      type: "operator"
    }
  ],
  [
    {
      text: "1",
      type: "number"
    },
    {
      text: "2",
      type: "number"
    },
    {
      text: "3",
      type: "number"
    },
    {
      text: "+",
      type: "operator"
    }
  ],
  [
    {
      text: "0",
      type: "number"
    },
    {
      text: "00",
      type: "number"
    },
    {
      text: ".",
      type: "number"
    },
    {
      text: "-",
      type: "operator"
    }
  ],
  [
    {
      text: "AC",
      type: "clear",
      style: "btn-light"
    },
    {
      text: "⌫",
      type: "back",
      style: "btn-light"
    },
    {
      text: "=",
      type: "total",
      style: "btn-large"
    }
  ]
];
export default {
  name: "Calculator",
  data() {
    return {
      display: "",
      currentValue: "" || "0",
      previousValue: "",
      currentOperator: null,
      buttons: buttons,
      operations: {
        "+": (a, b) => a + b,
        "-": (a, b) => a - b,
        "×": (a, b) => a * b,
        "÷": (a, b) => a / b
      }
    };
  },
  methods: {
    buttonClick(button) {
      let vm = this;
      switch (button.type) {
        case "number":
          vm.number(button);
          break;
        case "clear":
          vm.clearData();
          break;
        case "operator":
          vm.operator(button);
          break;
        case "total":
          vm.total(button);
          break;
        case "back":
          vm.back();
          break;
        default:
          vm.display += button.text;
          break;
      }
    },
    updateDisplay(value) {
      let vm = this;
      vm.currentValue = value;
      vm.$nextTick(function() {
        let fontSize = 56;
        vm.$refs.current.setAttribute("style", "font-size:56px");
        while (
          vm.$refs.currentValue.offsetWidth + 60 >
          vm.$refs.current.offsetWidth
        ) {
          vm.$refs.current.setAttribute(
            "style",
            "font-size:" + fontSize + "px"
          );
          fontSize -= 0.1;
        }
      });
    },
    number(button) {
      let vm = this;
      if (vm.previousValue === null) {
        vm.previousValue = vm.currentValue;
        vm.updateDisplay("");
      }
      if (
        button.text == "." &&
        vm.currentValue.includes(".") &&
        vm.display.includes(".")
      ) {
        return false;
      }
      vm.display += button.text;
      vm.updateDisplay(vm.currentValue + button.text);
      if (vm.display.slice(0, 2) == "00" && vm.display.length > 1) {
        vm.display = vm.display.slice(2);
        vm.currentValue = vm.display.slice(2);
        vm.updateDisplay(vm.currentValue);
      }
    },
    clearData() {
      let vm = this;
      vm.display = "";
      vm.previousValue = "";
      vm.currentValue = "";
      vm.updateDisplay(0);
    },
    operator(button) {
      let vm = this;
      if (vm.currentOperator) {
        vm.currentValue = vm.operations[vm.currentOperator](
          parseFloat(this.previousValue),
          parseFloat(this.currentValue)
        );
        vm.updateDisplay(vm.currentValue);
      }
      vm.display += button.text;
      vm.previousValue = null;
      vm.currentOperator = button.text;
    },
    total() {
      let vm = this;
      vm.currentValue = vm.operations[vm.currentOperator](
        parseFloat(this.previousValue),
        parseFloat(this.currentValue)
      );
      vm.updateDisplay(vm.currentValue);
      vm.display = vm.currentValue;
      vm.previousValue = null;
      vm.currentOperator = "";
    },
    back() {
      let vm = this;
      let str = vm.display;
      str = str.substring(0, str.length - 1);
      vm.display = str;
      vm.currentValue = str;
    }
  },
  filters: {
    currency(value) {
      var numeric = parseFloat(value);
      return numeric.toLocaleString("en-us");
    }
  }
};
</script>

<style lang="scss" scoped>
@import url("https://fonts.googleapis.com/css?family=Roboto+Condensed");
html,
div {
  margin: 0;
  padding: 0;
  border: 0;
  font-size: 100%;
}
.calculator-grid {
  font-family: "Roboto", sans-serif;
  user-select: none;
  width: 350px;
  height: 525px;
  border-radius: 20px;
  background-color: #062145;
  box-shadow: 0px 20px 40px rgba(0, 0, 0, 0.4);
}
.calculator-body {
  width: 100%;
  height: 416px;
  padding: 16px 0px;
  box-sizing: border-box;
}
.calculator-row {
  display: grid;
  width: 100%;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(1, 64px);
  grid-row-gap: 16px;
  padding-bottom: 16px;
  box-sizing: border-box;
}
.calculator-header {
  background-color: #041936;
  width: 100%;
  height: 109px;
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
  color: white;
  display: flex;
  justify-content: center;
  flex-direction: column;
  text-align: right;
  .calculator-str {
    padding-right: 16px;
    font-size: 16px;
    color: #00c4ff;
    font-weight: 300;
    overflow: hidden;
  }
  .calculator-sum {
    padding-right: 13px;
    font-size: 56px;
    font-weight: normal;
    line-height: 67px;
  }
}
.calculator-item {
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 24px;
  font-weight: 300;
  width: 72px;
  height: 64px;
  justify-self: center;
  align-self: center;
  cursor: pointer;
  border-radius: 16px;
  width: 72px;
  height: 64px;
  &:hover {
    background-color: #00c4ff;
  }
  &.num {
    font-weight: 300;
  }
  &.btn-operator {
    background-color: #041936;
    border-radius: 16px;
    width: 72px;
    height: 64px;
    justify-self: center;
    align-self: center;
    font-weight: 500;
    &:hover {
      color: #00c4ff;
    }
  }
  &.btn-light {
    font-weight: normal;
    color: #00c4ff;
    &:hover {
      color: white;
    }
  }
  &.btn-large {
    background: linear-gradient(to right, #00c4ff, #6c00ff);
    grid-column: 3 / span 2;
    width: calc(100% - 16px);
    display: flex;
    justify-content: flex-end;
    padding-right: 30px;
    box-sizing: border-box;
    border-radius: 16px;
    font-weight: 500;
    &:hover {
      color: #00c4ff;
    }
  }
}
</style>
