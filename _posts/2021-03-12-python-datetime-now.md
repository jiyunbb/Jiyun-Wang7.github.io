---
author: Jiyun Wang
layout: post
title: datetime.now() 명시적으로 사용하기
tags: [python]
---

일부 서비스 상에서 datetime.now()로 시간의 타임존을 naive 하게 사용하고 있었는데, 명시적으로 사용해야 한다는 코드 리뷰를 받고, datetime now가 어떻게 동작하는지에 대해서 간단하게 조사해보게 되었다.
<br><br>
확인해본 결과, datetime 자체가 기본적으로 타임존을 가지고 있지 않는 것으로 보인다.
<br>

아래 로컬에서 확인해보자. <br>
![example_1)](/assets/python-datetime-now/1.png)

<br><br>
예를 들어 datetime.now()에 타임존(datetime.now(timezone('Asia/Seoul')))을 지정하게 되면 타임존 속성을 가진 객체가 리턴이 되고, datetime.now()을 쓰는 경우 타임존 속성이 없는 객체가 리턴이 되는 것으로 확인된다.

<br><br>
즉, naive 하게 사용하는 경우 동작하는 환경의 서버 시간을 따르는 것으로 보이고, 서버 환경과 의존되지 않기 위해서는 명시적으로 표현하는 것을 습관화 해야할 것 같다. 그리고 기존에 naive 하게 쓰이는 곳은 어떻게 처리할지 고민이 필요할 것 같다.

<br><br>

### 적용되어 있는 로컬 서버 시간 (KST)
---
![example_2)](/assets/python-datetime-now/2.png)

<br>

### 참고
---
[https://docs.python.org/ko/3/library/datetime.html](https://docs.python.org/ko/3/library/datetime.html)