# Handlebar.js

## 가장 기본적인 바인딩 구조

```html
<script id="entry-template" type="text/x-handlebars-template">
<table>
  <thead>
    <th>이름</th>
    <th>아이디</th>
    <th>메일주소</th>
  </thead>
  <tbody>
    {{#users}}
    <tr>
      <td>{{name}}</td>
      <td>{{id}}</td>
      <td>M<a href="mailto:{{email}}">{{email}}</a></td>
    </tr>
    {{/users}}
  </tbody>
</table>
</script>
```
+ {{}} 로 감싸진 부분이 데이터와 바인딩되는 부분이
+ {{#user}} 와 {{/users}} 이부분이 루프도는 부분임

```javascript

//핸들바 템플릿 가져오기
var source = $("#entry-template").html(); 

//핸들바 템플릿 컴파일
var template = Handlebars.compile(source); 

//핸들바 템플릿에 바인딩할 데이터
var data = {
      users: [
            { name: "홍길동1", id: "aaa1", email: "aaa1@gmail.com" },
            { name: "홍길동2", id: "aaa2", email: "aaa2@gmail.com" },
            { name: "홍길동3", id: "aaa3", email: "aaa3@gmail.com" },
            { name: "홍길동4", id: "aaa4", email: "aaa4@gmail.com" },
            { name: "홍길동5", id: "aaa5", email: "aaa5@gmail.com" }
        ]
}; 

//핸들바 템플릿에 데이터를 바인딩해서 HTML 생성
var html = template(data);

//생성된 HTML을 DOM에 주입
$('body').append(html);


```


## 가장 기본적인 사용자 정의 헬퍼

```html

<script id="entry-template" type="text/x-handlebars-template">
<table>
    <thead> 
        <th>이름</th> 
        <th>아이디</th> 
        <th>메일주소</th> 
    </thead> 
    <tbody> 
        {{#users}} 
        <tr> 
            <td>{{name}}</td> 
            <td>{{id}}</td> 
            
            {{!-- 사용자 정의 헬퍼인 email에 id를 인자로 넘긴다 --}}
            <td><a href="mailto:{{email id}}">{{email id}}</a></td> 
        </tr> 
        {{/users}} 
    </tbody> 
</table>
</script>

```
+ {{email id}} email 이라는 이름의 사용자 정의 헬퍼가 사용된것.
+ email 이라는 이름의 사용자정의 헬퍼를 호출하면서 파라미터로 id 값을 전달.
+ 주석은 {{!-- 주석내용 --}}


```javascript

//핸들바 템플릿 가져오기
var source = $("#entry-template").html(); 

//핸들바 템플릿 컴파일
var template = Handlebars.compile(source); 

//핸들바 템플릿에 바인딩할 데이터
var data = {
      users: [
            { name: "홍길동1", id: "aaa1" },
            { name: "홍길동2", id: "aaa2" },
            { name: "홍길동3", id: "aaa3" },
            { name: "홍길동4", id: "aaa4" },
            { name: "홍길동5", id: "aaa5" }
        ]
}; 

//커스텀 헬퍼 등록 (id를 인자로 받아서 전체 이메일 주소를 반환)
Handlebars.registerHelper('email', function (id) {
  return id + "@daum.net";
});

//핸들바 템플릿에 데이터를 바인딩해서 HTML 생성
var html = template(data);

//생성된 HTML을 DOM에 주입
$('body').append(html);


```

+ 바인딩할 데이터에 email 부분이 없다. 
+ 커스텀헬퍼를 등록!


## 기본 제공 헬퍼

```html

<table>
  <thead>
    <th>이름</th>
    <th>아이디</th>
    <th>메일주소</th>
    <th>요소정보</th>
  </thead>
  <tbody>
    {{!-- {{#each users}} 는 {{#users}} 로도 대체 가능하다  --}}

    {{#each users}}
    <tr>
    {{!-- {{name}}은 {{this.name}} 과 같고 {{.}}은 현재 name 과 id를 포함하고 있는 오브젝트를  가르킨다--}}
      <td>{{name}}</td>
      <td>{{id}}</td>

      {{!-- 사용자 정의 핼퍼인 email 에 id를 인자로 넘긴다 --}}
      <td><a href="mailto:{{email id}}">{{email id}}</a></td>

      {{!-- 요소의 손번은 @index 혹은 @key 로 잡을수 있는데, array 와  object 모두 잡을 수 있는 key 가 더 나아보인다 --}}

      {{#if @first}}
      <td> 첫아이템 {{@key}} 번재 요소 </td>
      
      {{else if @last}}
      <td> 마지막아이템 {{@key}} 번재 요소 </td>

      {{else}}
      <td> 중간 아이템 {{@key}} 번재 요소 </td>

      {{/if}}
    </tr>
    {{/each}}

  </tbody>
</table>

```


## partial template 등록 및 사용


```html
<!-- 핸들바 템플릿 -->

<script id="partial-template" type="text/x-handlebars-template">
  {{!-- #unless 핼처는 #f의 정반대 기능을 한다. --}}

  <h1> 리스트 {{#unless users}} <small>사용자 리스트가 없습니다.</small> {{/unless}} </h1>

</script>

<!-- 핸들바 템플릿 -->
<script id="partial-template" type="text/x-handlebars-template">
{{-- 조각 템플릿 #>를 사용해서 포함시킬수 있다 --}}

{{#> commonHeader}}
  partial template 로드 실패시 보여지는 내용
{{/commonHeader}}
<table>
    <thead> 
        <th>이름</th> 
        <th>아이디</th> 
        <th>메일주소</th> 
        <th>요소정보</th>
    </thead> 
    <tbody> 
        {{!-- {{#each users}} 는 {{#users}} 로도 대체 가능하다 --}}
        {{#each users}} 
        <tr> 
            <td>{{name}}</td> 
            <td>{{id}}</td> 
            
            {{!-- 사용자 정의 헬퍼인 email에 id를 인자로 넘긴다 --}}
            <td><a href="mailto:{{email id}}">{{email id}}</a></td> 
            
            {{!-- 요소의 순번은 @index 혹은 @key로 잡을 수 있는데,
            array와 object 모두 잡을 수 있는 key가 더 나아보인다 --}}
            
            {{#if @first}}
                <td>첫 아이템 ({{@key}} 번째 요소)</td>
            {{else if @last}}
                <td>마지막 아이템 ({{@key}} 번째 요소)</td>
            {{else}}
                <td>중간 아이템 ({{@key}} 번째 요소)</td>
            {{/if}}
        </tr> 
        {{/each}}
    </tbody> 
</table>


</script>

```

```javascript

//핸들바 조각 템플릿 가져오기
var partial = $("#partial-template").html();

//핸들 템플릿 가져오기
var source = $("#partial-template").html();

//핸들바 템블릿 컴파일
var template = Handlebars.compile(source);

//핸들바 넴플릿에 바인딩할 데이터
var data = {
    users: [
      {name: "홍길동1", id: "aaa1"},
      {name: "홍길동2", id: "aaa2"},
      {name: "홍길동3", id: "aaa3"},
      {name: "홍길동4", id: "aaa4"},
    ]

};

//조각 템플릿을 'commonHeader'라는 이름으로 등록
  Handlebars.registerPartial('commonHeader', partial);

//커스텀 핼퍼 등록 (id를 인자로 받아서 전체 이메일 주소를 반환)
  Handlebars.registerHelper('email', function(id) {
    return id + "@daum.net";
  };

//핸들바 템플릿에 데이터를 바인행해서 HTML로 생성된
  var html = template(data);

//생성된 HTML을 DOM에 주입
  $(body).append(html);

```