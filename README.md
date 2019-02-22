# JavaScript 題目篇 - 新手 JS 地下城
3F - 計算機
 <a href="https://huiyuliz.github.io/calculator/" target="_blank">完成品</a>、
 <a href="https://github.com/HuiyuLiz/calculator/blob/master/src/components/Calculator.vue" target="_blank">程式碼</a>
 
 使用 Vue.js 進行破關，計算機畫面用 CSS Grid 排版。   
 while - 當輸入數字過多時，進行字體大小伸縮。  
 [Vue.js Filters](https://vuejs.org/v2/guide/filters.html) - 搭配 [toLocaleString](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString) 來將數字格式轉換成千分位。  
```html
 <div>{{ 3500000 | currency }}</div> 
<!--顯示3,500,000-->
```     
```Vue.js
filters: {
  currency(value) {
    var numeric = parseFloat(value);
    return numeric.toLocaleString("en-us");
  }
}
```
 
 
 # 參考教學
freeCodeCamp.org [Build a Calculator with Vue.js](https://www.youtube.com/watch?v=m1_ih43p24s)  
Coding Garden with CJ [Build a Calculator with Vue.js](https://www.youtube.com/watch?v=MRsDx3sFKOs)


