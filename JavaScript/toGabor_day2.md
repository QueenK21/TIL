Day 2: 온라인 실험에서 Gabor Patch 만들기 
===========================================


Day 1에서 [Online Gabor-patch generator](https://www.cogsci.nl/gabor-generator)를 찾아 php 가보를 만들려고했는데, js로 짠 [gaborgen](https://github.com/jtth/gaborgen-js)을 찾음..ㅠ 이걸로 만드는게 더 쉽고 빠를 것 같고 git pages에선 php지원이 어차피 안된다고하니 gaborgen을 이용해서 가보 제시 실험dmf 만들기로했다. 근데 문제는 이 스크립트가 jQuery를 사용하고있는데 jQuery문법이 나에게 너무나 생소하다...오늘 붙들고있는 내내 끙끙 앓았는데 조금씩 앞으로 나가보자. 
   
__*UTF-8__   
```html
<meta charset="utf-8">
```
jsPsych tutorial에는 이 부분이 없어서 처음 이걸 보고 이게 뭐지? 했는데, 
\<meta> 웹페이지에 대한 메타데이터를 포함하는 html 태그   
\<charset> 웹페이지 문자 인코딩 정의 
\<utf-8> 거의 세상의 모든 언어 디스플레이 가능하게 만들어줌   
의 조합으로, utf-8 이전 ASCII일 때는 영어만 인코딩 가능했다고 한다.       
html5 표준에서는 다행히 기본 설정이 utf-8이라고 하는데, 그래도 혹시 모르니 항상 넣어주는게 좋다고.  

__*$()__   
처음 보자마자 당황했다 도대체 이게 뭔 형식인가하고...아래는 jQuery공식 페이지에 있는 설명이다.
```JavaScript
window.onload = function(){
    alert("welcome");
};
```   
js에서 웹브라우저가 html document를 다 로드하고 코드를 돌리게 하기 위해선, window.onlod 함수로 위와 같이 감싸줘야한다고 한다. 근데 이렇게 하면(?) 배너 광고를 포함한 모~든 이미지가 다 다운로드될 때 까지 코드가 돌아가지 않는다고 한다.   
내가 이해한 바로는, 배너 광고 같이 코드와 무관한 것 까지 모두 로딩되기를 기다리지 않고 코드가 돌아가기에 충분한 정보가 로드되고나면 바로 돌아가도록 하기 위해 jQuery에서는 아래와 같이 표현한다.   
```JavaScript
$(document).ready(function(){
    //*Your code here*
})
```   
> 여기서 날 가장 당혹시킨 $는 그냥 jQuery의 alias  

gaborgen에서 제공하고있는 가보를 슬라이드바로 조절하여 보여주는 index.html 파일에서 처음 나타나 나를 당혹시킨,   
```JavaScript
$(function(){
    //*Your code here*
})
```   
은 바로 위의 $(document).ready(function(){})의 짧은 버전일 뿐이라고 한다.  

__*$("#name")__   
그 다음 나를 당혹시킨 것은 #. #은 id다. w3schools html 파트 예시 문제에서 처음 나오는 그 id! id는 html문서에서 고유한 영역을 식별하는 역할이라고 하는데, 스타일 시트에서 스타일을 지정하거나, js에서 element들을 조작하기 위해 쓰는 거라고 한다. element가 뭔지 헷갈리니, w3schools 예시를 가져와보자.   
```html
<h1 id="myHeader">Hello World!</h1>
<button onclick="displayResult()">Change text</button>

<script>
function displayResult() {
    document.getElementById("myHeader").innerHTML = "Have a nice day!";
}
</script>
```   
```markdown
{%include innerHTML_test.html id="v=CbwkwncIyH8&feature=youtu.be"%}
```    
myHeader라는 id가 존재하고, 아래서 getElementById로 myHeader 의 element를 불러와 버튼을 클릭하면 Have a nice day!가 출력되게 만드는 것이다. 즉, myHeader 요소인 Hello World (=innerHTML) 을 Have a nice day로 재세팅한 것. Have a nice day라고 새로 부여하지 않고, 
var contet = document.getElementById("myHeader").innerHTML; alert(content); 하면, alert에 원래 myHeader의 innerHTML인 Hello World!를 띄워준다.  




