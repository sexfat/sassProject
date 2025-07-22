# Sass Mixin 說明文件

## 什麼是 Mixin？
Mixin 是 Sass 中的一個強大功能，允許您定義可重複使用的樣式規則組合。使用 `@mixin` 定義，使用 `@include` 引用。

## 基本語法

### 定義 Mixin
```scss
@mixin mixin-name {
  // CSS 規則
}
```

### 使用 Mixin
```scss
.selector {
  @include mixin-name;
}
```

## 專案中的 Mixin 程式碼記錄

### 完整程式碼內容
```scss
//宣告
@mixin center {
    margin: auto;
    width: 400px;
}

//引用
.box {
    @include center;
    height: 300px;
    background-color: #ff0000;
}

.box1 {
    @include center;
    height: 700px;
    background-color: #00d5ff;
}

//變數 宣告方式
@mixin model-center($w) {
    margin: auto;
    width: $w;
}

.box2 {
    @include model-center(700px);
    height: 300px;
    background-color: #900043;
}

//layout ->預設值
@mixin layout($w , $bgc: #fff) {

    //滿版 
    @if $w ==100 {
        width: 100%;
        height: auto;
    }

    @else if $w < 100 {
        width: $w * 1%;
        margin: auto;
    }

    @else {
        //固定寬
        @include model-center($w)
    }

    background-color: $bgc;
}

section.intro {
    @include layout(90, #333);
    height: 400px;
}

// 形狀
@mixin rect($w , $h: $w) {
    width: $w;
    height: $h;
}

.box4 {
    @include rect(600px);
}

// 標題
@mixin title($fontSize) {
    font-size: $fontSize;
    font-weight: bold;
    width: 100%;
    min-width: 350px;
    text-align: center;
    margin: auto;
    font-family: Verdana, Geneva, Tahoma, sans-serif;

    &::before {
        content: '|';
        color: #fff;
    }
    &::after {
        content: '|';
        color: #fff
    }
}

// 標籤寫在外部
h1.Maintitle {
    @include title(100px);
}

// 圓角
@mixin bordeRadius($w , $h , $b){
    border-radius: $b;
    @include rect($w, $h) 
}

.card {
  @include bordeRadius(430px, 140px, 10px);
  background-color: #900043;
  box-shadow: 1px 1px 10px #ffffff;
}
```

## 專案中的 Mixin 範例

### 1. 基本置中 Mixin
```scss
@mixin center {
    margin: auto;
    width: 400px;
}

// 使用方式
.box {
    @include center;
    height: 300px;
    background-color: #ff0000;
}
```

### 2. 帶參數的 Mixin
```scss
@mixin model-center($w) {
    margin: auto;
    width: $w;
}

// 使用方式
.box2 {
    @include model-center(700px);
    height: 300px;
    background-color: #900043;
}
```

### 3. 帶預設值的 Mixin
```scss
@mixin layout($w, $bgc: #fff) {
    // 滿版處理
    @if $w == 100 {
        width: 100%;
        height: auto;
    }
    // 百分比寬度
    @else if $w < 100 {
        width: $w * 1%;
        margin: auto;
    }
    // 固定寬度
    @else {
        @include model-center($w);
    }
    
    background-color: $bgc;
}

// 使用方式
section.intro {
    @include layout(90, #333);
    height: 400px;
}
```

### 4. 形狀 Mixin
```scss
@mixin rect($w, $h: $w) {
    width: $w;
    height: $h;
}

// 使用方式 - 正方形（只傳一個參數）
.box4 {
    @include rect(600px);
}
```

### 5. 標題樣式 Mixin
```scss
@mixin title($fontSize) {
    font-size: $fontSize;
    font-weight: bold;
    width: 100%;
    min-width: 350px;
    text-align: center;
    margin: auto;
    font-family: Verdana, Geneva, Tahoma, sans-serif;

    &::before {
        content: '|';
        color: #fff;
    }
    &::after {
        content: '|';
        color: #fff;
    }
}

// 使用方式
h1.Maintitle {
    @include title(100px);
}
```

### 6. 複合 Mixin（調用其他 Mixin）
```scss
@mixin bordeRadius($w, $h, $b) {
    border-radius: $b;
    @include rect($w, $h);
}

// 使用方式
.card {
    @include bordeRadius(430px, 140px, 10px);
    background-color: #900043;
    box-shadow: 1px 1px 10px #ffffff;
}
```

## Mixin 的優點

1. **代碼重用性**：避免重複寫相同的 CSS 規則
2. **參數化**：可以傳遞參數來自定義行為
3. **條件邏輯**：可以使用 `@if`、`@else` 等條件語句
4. **組合性**：Mixin 可以調用其他 Mixin
5. **維護性**：修改 Mixin 會影響所有使用它的地方

## 最佳實踐

1. **命名規範**：使用描述性的名稱
2. **參數順序**：必需參數在前，可選參數在後
3. **預設值**：為可選參數提供合理的預設值
4. **文檔註解**：為複雜的 Mixin 添加註解說明
5. **單一職責**：每個 Mixin 應該專注於一個特定功能

## 與函數的區別

- **Mixin**：輸出 CSS 規則，使用 `@include` 調用
- **Function**：返回值，使用 `@return`，在屬性值中調用

## 編譯結果

Mixin 會在編譯時將其內容複製到調用的位置，生成最終的 CSS 代碼。