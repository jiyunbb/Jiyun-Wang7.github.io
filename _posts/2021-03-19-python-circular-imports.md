---
author: Jiyun Wang
layout: post
title: python circular imports
categories: python
---

### 순환 참조란?
---
두 개의 모듈이 서로를 참조하는 경우 발생합니다. 예를 들어 A 가 B를 참조하고, B가 A를 참조하는 경우를 말합니다.


### 잘못된 예시
---

![잘못된 예시](/assets/python-circular-imports/1.png)

![run debug (with breakpoint)](/assets/python-circular-imports/5.png)

**run a.py 결과**

![a.py 결과](/assets/python-circular-imports/2.png)

**run b.py 결과**

![b.py 결과](/assets/python-circular-imports/3.png)


### 왜 에러가 났을까?
---

a 임포트하면 b 임포트하고, 다시 b 에서 a 를 임포트 하는 순환 구조 때문에 에러가 발생할 것 같지만, Python은 한번 임포트된 것을 다시 임포트하진 않는다. import time 과 runtime을 확인해보자.


### import time 과 runtime
---

Python은 프로그램 실행시 import time과 runtime이 구분되어 실행된다. 설명은 대략 아래와 같음.

- rumtime :  실제로 모든 코드가 실행되는 순간
- import time : .py 모듈에 들어 있는 소스 코드를 위에서 부터 순서대로 한 번 파싱하고, 실행을 위한 바이트 코드를 생성한다. import time의 결과물로 .pyc 파일이 생성되며, Syntax Error 도 코드를 파싱하는 과정에서 일어난다. 또한 함수 본문과 클래스의 메소드 본문을 제외한 대부분의 코드를 import time에 실행한다. (최신 .pyc 파일이 존재할 경우 이 과정이 생략)

즉, import time에는 함수 본문과 클래스 메소드 본문을 제외한 코드들을 실행하는데 이 부분이 [a.py](http://a.py/) 실행시 문제가 되어 순환 참조 에러가 발생한다. a.py의 경우 function_a() 코드가 import time에 실행되기 때문에 a 모듈과 b 모듈이 반복적으로 import 되기 때문이다.


**개선된 코드 (9라인 확인)**

![개선된 코드](/assets/python-circular-imports/4.png)


### 해결 방법
---

모듈을 import 하는 시점을 import time이 아닌 runtime으로 옮기면 된다. a.py 의 function_a() 가 import time에 호출되기 때문에 function_a() 이후 문제가 되는 b.py 의 `import a` 부분을 runtime으로 옮긴다. 즉, a를 직접 사용하고 있는 function_c() 로 옮기면 된다. 다른 상황에서 영향을 받지 않도록

지금 예시는 간단한 것이라서 알아차리기가 쉽지만, 파일 구조가 많고, 복잡한 경우 (현재는 commerce-front-api에 있는 것으로 알고있음.) 에는 알아차리기가 어려울 수도 있을 것 같다.


### 그 밖의 해결법 (이건 더 자세히 찾아보자)
---

Python typing type hint 를 위해 기존보다 더 많은 import 가 발생하면서 순환 참조가 발생할 확률이 높아집니다. 이럴 때는 typing 모듈에서 제공하는 TYPE_CHECKING 이라는 값을 활용하면 해결할 수 있습니다.

관련 블로그 확인 : [https://item4.blog/2017-12-03/Resolve-Circular-Dependency-on-Python-Typing/](https://item4.blog/2017-12-03/Resolve-Circular-Dependency-on-Python-Typing/)


### 참고
---

[https://blog.naver.com/PostView.nhn?blogId=topblade71&logNo=221549872542](https://blog.naver.com/PostView.nhn?blogId=topblade71&logNo=221549872542)

[https://medium.com/mathpresso/python-circular-imports-e89c5bf16510](https://medium.com/mathpresso/python-circular-imports-e89c5bf16510) (내가 실습하며 이해했던 곳 - 본문 내용)

[https://stackabuse.com/python-circular-imports/](https://stackabuse.com/python-circular-imports/)

[https://velog.io/@lololtoday/pythoncircular-import-problem](https://velog.io/@lololtoday/pythoncircular-import-problem)