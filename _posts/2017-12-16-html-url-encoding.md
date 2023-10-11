---
author: Jiyun Wang
layout: post
title: HTML URL Encoding(Percent Encoding)
tags: [html, javascript]
---

http://www.google.co.kr/소설.html <br>
-> HTML에서 이런 한글이 섞인 주소는 오작동할 수 있기에

http://www.google.co.kr/%EC%86%8C%EC%84%A4.html <br>
-> 이런 식으로 인코딩 해주어야 한다.

다음과 같은 3가지 함수 중 하나로 한글 주소를 인코딩할 수 있다.

* **encodeURI() / decodeURI()** : 
최소한의 문자만 인코딩한다.
; / ? : @ & = + $ , - _ . ! ~ * ' ( ) #
이런 문자는 인코딩하지 않는다.
http:// ... 등은 그대로 나온다.

* **encodeURIComponent() / decodeURIComponent()** : 
알파벳과 숫자 Alphanumeric Characters 외의, 대부분의 문자를 모두 인코딩한다.
http:// ... 가 http%3A%2F%2F 로 된다.

* **escape() / unescape()** : 
encodeURI() 와 encodeURIComponent() 의 중간 정도의 범위로 문자를 인코딩한다.
대부분의 경우에서 encodeURI() 가 적당하다.
다만, 주소 전체를 http://부터 모두 인코딩하기 위해서는 encodeURIComponent 를 사용한다.

인코딩된 한글 주소를 다시 복원하기 위해서는 각각의 함수에 대응되는 디코딩 함수를 사용한다.<br><br>
-> 인터넷 주소창 등에서, 많은 퍼센트(%) 기호들을 볼 수 있다. 알파벳과 숫자가 아닌, 특수 문자나 한글이 인코딩되어 있는 것이다. 이것을 해독하기 위해서는 디코딩(decoding) 과정을 거쳐야 한다.<br><br>
-> 예를 들어, %EC%86%8C%EC%84%A4를 decodeURI() 함수로 디코딩하면 소설이라는 문자열이 나타난다. 그러나 만약 escape() 함수로 인코딩한다면, 소설이라는 문자열이 %uC18C%uC124 이렇게 표현된다. 이것은 unescape() 함수로 풀어야 한다. encodeURIComponent() 함수는 encodeURI() 함수보다, 더 넓은 범위의 문자들을 인코딩하는 함수이기 때문이다.

### 예제 
---
```html
<html>
<body>

<script type="text/javascript">
  var s;

  s = encodeURI('http://www.google.co.kr/소 설.html');
  document.write('<p>' + s + '<p>');
  // 출력 결과: http://www.google.co.kr/%EC%86%8C%20%EC%84%A4.html


  s = encodeURIComponent('http://www.google.co.kr/소 설.html');
  document.write('<p>' + s + '<p>');
  // 출력 결과: http%3A%2F%2Fwww.google.co.kr%2F%EC%86%8C%20%EC%84%A4.html


  s = escape('http://www.google.co.kr/소 설.html');
  document.write('<p>' + s + '<p>');
  // 출력 결과: http%3A//www.google.co.kr/%uC18C%20%uC124.html
</script>

</body>
</html>
```
어떤 함수든 "공백 문자" 즉 스페이스는 %20 으로 치환한다. 그러나 되도록 주소의 공백은 없어야 한다.
<br><br>
## 참고
---
[http://mwultong.blogspot.com/2006/10/urlencode-encoding-javascript.html](http://mwultong.blogspot.com/2006/10/urlencode-encoding-javascript.html)

[https://www.w3schools.com/tags/ref_urlencode.asp](https://www.w3schools.com/tags/ref_urlencode.asp)
