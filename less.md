# {Less}  
  
&nbsp; &nbsp; [嵌套规则](#嵌套规则)  
&nbsp; &nbsp; [避免编译](#避免编译)  
&nbsp; &nbsp; [变量](#变量)  
&nbsp; &nbsp; [运算](#运算)  
&nbsp; &nbsp; [混合](#混合)  
&nbsp; &nbsp; [带参数混合](#带参数混合)  
&nbsp; &nbsp; [Mixins函数](#Mixins函数)  
&nbsp; &nbsp; [模式匹配](#模式匹配)  
&nbsp; &nbsp; [导引表达式](#导引表达式)  
  
  
### 嵌套规则
``` 
    // html
    <header>
        <h1>Hello Word!</h1>
        <p>Welcome to learn <span>Less</span>!</p>
    </header>
    
    // less
    header{
        background-color: #eeeeee;
        h1{
            font-size: 24px;
            color: #f50000;
        }
        p{
            font-size: 12px;
            span{
                color: blue;
                font-size: 16px;
                font-weight: 600;
            }
        }
    }

    // css
    header {
        background-color: #eeeeee;
    }
    header h1 {
        font-size: 24px;
        color: #f50000;
    }
    header p {
        font-size: 12px;
    }
    header p span {
        color: blue;
        font-size: 16px;
        font-weight: 600;
    }
```

### 避免编译  
    需要输出一些不正确的CSS语法或者使用一些 LESS不认识的专有语法时,可以在字符串前加上一个 ~;
    在将less代码编译为css代码之后,〜"some_text"中的任何内容将显示为 some_text;
```
    // less
    span{
        color: ~"green";
    }

    // css{
        color: green;
    }
```

### 变量  
    允许我们单独定义一系列通用的样式,然后在需要的时候去调用;甚至可以用变量名定义为变量;
    字符串插值:@{变量名};
```
    // less
    @url: "../images/";
    @color: #f50000;
    @color2: "color";
    h1{
        color: @color;
    }
    p{
        color: @@color2;
        background-image: url('@{url}bg.png');
    }

    // css
    h1{
        color: #f50000;
    }
    p{
        color: #f50000;
        background-image: url('../images/bg.png');
    }
```

### 运算  
    (+,-,*,/)任何数字、颜色或者变量都可以参与运算;并且它能够分辨出颜色和单位;
```
    // less
    @font12: 12px;
    p{
        font-size: @font12 * 2;
        color: #f50000 + #0000ff;
    }

    // css
    p{
        font-size: 24px;
        color: #f500ff;
    }
```

### 混合  
    less可以定义一些通用的属性集为一个class,然后在另一个class中去调用这些属性;
    (任何 CSS class, id 或者 元素 属性集都可以以同样的方式引入)
```
    // less
    .border{
        border: solid 2px #ff5000;
    }
    .box{
        .border;
    }

    // css
    .border{
        border: solid 2px #ff5000;
    }
    .box{
        border: solid 2px #ff5000;
    }
```

### 带参数混合  
```
    // less
    .border (@color){
        border: 2px solid @color;
    }

    .box{
        .border(#ff5000);
    }

    // css
    .box {
        border: 2px solid #ff5000;
    }
```

还可以给参数设置默认值;
```
    // less
    .border (@color: red){
        ...
    }
```

    也可以定义不带参数属性集合,如果你想隐藏这个属性集合,不让它暴露到CSS中去,但是你还想在其他的属性集合中引用;
```
    // less
    .border () {
        ...
    }
    .box{
        .border;
    }

    // css
    .box {
        ...
    }
```

@arguments 变量(包含了所有传递进来的参数)
```
    // less
    .border(@size, @line, @color) {
        border: @arguments;
    }
    .box{
        .border(2px, solid, #ff5000)
    }

    // css
    .box {
        border: 2px solid #ff5000;
    }
```
### Mixins函数  
    描述:由变量和混合组成的混合可以在调用者的作用域中使用,并且是可见的; 但是有一个例外,如果调用者包含具有相同名称的变量,那么该变量不会复制到调用者的作用域中; 只有调用者范围内的变量被保护,并且继承的变量将被覆盖;

```

```


### 模式匹配  

模式匹配: 根据传入的参数来改变混合的默认呈现  
```
    // less
    // 满足dark条件执行 
    .mixin(dark, @color){
        color: darken(@color, 10%);
    }
    // 满足light条件执行 
    .mixin(light, @color){
        color: lighten(@color, 10%);
    }
    // @_接受如何参数
    .mixin(@_, @color){
        display: block;
    }

    @switch: light;
    .long{
        .mixin(@switch, #888)
    }
    @switch2: dark;
    .meng{
        .mixin(@switch2, #666)
    }

    // css
    .long {
        color: #a2a2a2;
        display: block;
    }
    .meng {
        color: #4d4d4d;
        display: block;
    }
```
### 导引表达式  
导引表达式: 通过导引混合而非if/else语句来实现条件判断  
```
    // less
    // when关键字用以定义一个导引序列  
    .mixin(@color) when (lightness(@color) >= 50%) {
        background-color: #000;
    }
    .mixin(@color) when (lightness(@color) < 50%) {
        background-color: #fff;
    }
    .mixin(@color) {
        color: @color;
    }

    // css
    .class1 {
        .mixin(#ddd)
    }
    .class2 {
        .mixin(#555)
    }
```
可用的比较运算: >  >=  =  =<  <  布尔值(除去关键字true以外的值都被视示布尔假);  


导引序列使用逗号','—分割,只有所有条件都符合时,才会被视为匹配成功;  
```
    .mixin (@a) when (@a > 0), (@a < 10){
        ...
    }
```
在导引序列中可以使用 'and' 关键字实现 '与' 条件, 使用 'not' 关键字实现 '或' 条件;  
```
    .mixin (@a) when (isnumber(@a)) and (@a > 0) { ... }
    .mixin (@a) when (isnumber(@a)) { ... }
```

导引可以无参数, 也可以对参数进行比较运算;  
```
    @media: mobile;
    .mixin (@a) when (@media = mobile) { ... }
    .mixin (@a) when (@media = desktop) { ... }

    .max (@a, @b) when (@a > @b) { width: @a; } 
    .max (@a, @b) when (@a < @b) { width: @b; } 
```

基于值类型匹配, 可以使用is*函式;  
常见的检测函式: iscolor, isnumber, isstring, iskeyword, isurl  
```
    .mixin (@a) when (isnumber(@a)) { ... }
    .mixin (@a) when (iscolor(@a)) { ... }
```













