> 收集各种前端编程的奇技淫巧

## 数字千位分隔符

数字需要用括号包裹，否则解析器会将`.`解析为小数点，报语法错误。也可以写成`n..toLocaleString()`，这样解析器会认为第一个点是小数点，第二个点是属性访问器。

大部分国家的数字本地格式都是用逗号作千位分隔符。但也有例外，比如`(4374389489327893).toLocaleString('de-DE')`的返回值就是`4.374.389.489.327.893`，这是德国的格式。再比如`(4374389489327893).toLocaleString('ar-SA')`的返回值就是`٤٬٣٧٤٬٣٨٩٬٤٨٩٬٣٢٧٬٨٩٣`，这是沙特阿拉伯的格式。

```javascript
function kilofy(n) {
    return (n).toLocaleString('zh-CN');
}

kilofy(4374389489327893);

// "4,374,389,489,327,893"
```

🌝🌖🌗🌘🌑🌒🌓🌔🌚

为什么要加一个零宽否定先行断言`(?!^)`？它的意思是匹配项不能在行首之前。

一般情况下，`/(?=(\d{3})+$)/g`就可以达到效果。但是也有特殊情况，比如`374389489327893`就会返回`,374,389,489,327,893`，而这就是零宽否定先行断言发挥作用的场景。

```javascript
const reg = /(?!^)(?=(\d{3})+$)/g;

function kilofy(n) {
    return String(n).replace(reg, ',');
}

kilofy(4374389489327893);

// "4,374,389,489,327,893"
```

## 格式化 URL 查询字符串为对象

replace 回调的后两个参数 k 和 v，分别对应正则中的两个捕获组。

```javascript
const reg = /([^?&=]+)=([^&]+)/g;

function getQueryStringObject() {
    const q = {};
    location.search.replace(reg, (m, k, v) => q[k] = v);
    return q;
}

getQueryStringObject();

// URL: https://matiji.cn/path?a=z&b=y&c=x
// { a: z, b: y: c: x }
```
