# BEM (Block Element Modifier)

+ Block 영역짓고
+ Element 로 지정하고
+ Modifier 상태표시


```
.block {}
.block__elemnet {} 명사와 같은 이름 (__button, __title, __menu)
.block--modifier {} 동사과 같은 상태 (--active, --disabled, --full)

```

```html
<div class="search">
  <button class="search__btn search__btn--active">BUTTON</button>
  <button class="search__btn search__btn--disable">BUTTON</button>
</div>

```

* 먼저 하나의 구성요소를 지정함 (.팝업 {})
* 구성요소에 Child, Element, Component 들이 있다면 이름을 명시 (.팝업_제목 {})
* 구성요소가 하는일이 있다면 -- 로 지정(.팝업--오픈 {})

```
.팝업 {}
.팝업__제목 {}
.팝업--오픈 {}
.팝업--오픈__제목 {}
.팝업__제목--오른쪽__버튼 {}

```

## 비교
+ 일반적

```html
<div class="popup full open">
  <h1 class="title">타이틀</h1>
  <div class="body">
    
    //내용
  </div>
</div>
```
+ BEM방식

```html

<div class="popup-wrap popup-wrap--full popup-wrap--full">
  <h1 class="popup-wrap__title">타이틀</h1>
  <div class="popup-wrap__body">
    내용
  </div>

</div>

```

```html

<div class="notice">
  <img class="notice__img--rev" src="logo.png"></img>  

  <div class="notice__body">
    <h3 class="title">타이틀</h3>
    <p class="text"> 설명글 </p>
  </div>
</div>

```




