Day 3: 온라인 실험에서 Gabor Patch 만들기 
===========================================  


올라와있는 gaborgen 함수에서 rescale과 같은 변수가 뭘 의미하는지 모르겠어서, matlab에서 쓰던 gabor function 을 JS로 옮기는 작업을 하는 중.  


**-Array의 element에 연산하기**  
```javascript
array = [1, 2, 3];
array_new = array.map(value => (value + 3) / 2);
console.log(array_new); // [2, 2.5, 3]
```  
element 각 값에다가 특정 수들을 연산할 때는 위와 같이 .map 을 사용한다. 'value'는 어떤 이름이든 상관 없다. array 안의 element를 지정하는 부분.  



  
**-Array에 새 element들 추가하기**  
```javascript
add_array = [1, 2, 3];
array = [];
for (i=0; i < 4; i++){
  	add_new = add_array.map(value=>value+i);
    array.push(add_new);
}
console.log(array); // [[1,2,3],[2,3,4],[3,4,5],[4,5,6]]
```  
기존 만들어 둔 array에 새 요소들을 추가하고 싶을 때는 위와 같이 push를 쓴다. 나는 아직 쓰지 않았으나, 요소를 추가하여 새로운 array를 만들때는 concat를 사용한다. 즉, push는 위와 같이 사용하고, concat는 newArray = array.concat(add_array) 와 같이 새로운 배열로 아웃풋을 받아서 사용한다.  




**-Function declaration VS expression**  
-Function declaration   
```Javascript
function name(param1, param2, ..., paramN){
    //your code 
}
```   
이 경우, function이 hoist되기 때문에, function이 스크립트 상에서 선언되기 전에 함수를 사용할 수 있다.  

-Function expression   
```JavaScript
name = function(param1, param2, ..., paramN){
    //your code
}
```   
이 경우, 반드시 이 함수가 먼저 선언되어야 함수를 사용하는 것이 가능하다. 즉 스크립트 상에서 이 함수가 먼저 나와있고 뒤에서 이 함수를 사용한다는 것. Function declaration은 코드 내내 사용할 함수일 때 쓰면 좋고, 순차적으로 function 1 -> function 2 -> function 3 이런식으로 이어지게 만들 경우에는 (즉, 함수를 콜백으로 사용할 경우에는) function expression을 쓰면 좋다. matlab만 쓰던 내게는 function expression이 훨씬 이해하기 쉽다.  



**-함수에 variable이 들어오지 않았을 때**   
코드 짤 때 특히 함수를 짤 때 입력값으로 여러 variable들을 설정해놓고 특정 값들은 ommit해도 되는 요소로 만들고 싶을 때가 있다. 그럴 때는,   
```javascript
var name = function(param1, param2, param3){
    if (typeof param3 === 'undefined' || param3 === 'null'){
        param3 = your_defaultValue; 
    }
}
```   
이런식으로 하면 해당 값이 들어오지 않았을 때 부여할 default 값을 설정할 수 있다. 검색하다보니 default를 부여하는 더 쉬운 방법도 있는 거 같은데, 모든 브라우저에서 에러 없이 돌아가도록 할 때는 위와 같은 방법이 안전하다는 것 같다. 
