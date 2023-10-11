---
author: Jiyun Wang
layout: post
title: setUpTestData() vs setUp()
tags: [python, test]
---

Django에서 테스트 메소드를 수행하기 전에 테스트 데이터를 세팅하는 메소드를 사용하여 테스트 데이터를 만들어야 한다.

```python
@classmethod
def setUpTestData(cls):
    print("setUpTestData: Run once to set up non-modified data for all class methods.")
    pass

def setUp(self):
    print("setUp: Run once for every test method to setup clean data.")
    pass
```

- `setUpTestData()` 는 클래스 전체에서 사용되는 설정을 위해서 **테스트 시작 때 딱 한 번만 실행**됩니다. 테스트 메쏘드가 실행되면서 수정되거나 변경되지 않을 객체들을 이곳에서 생성할 수 있습니다.
- `setUp()` 은 **각각의 테스트 메소드가 실행될 때마다 실행**됩니다. 테스트 중 내용이 변경될 수 있는 객체를 이곳에서 생성할 수 있습니다.

테스트 메소드에서 데이터 변경(추가/수정/삭제)이 일어나는지 확인하고 적절하게 데이터 셋업 메소드를 선택해야 한다. 그렇지 않으면 각 테스트 간의 독립성이 보장이 되지 않고 영향을 줄 수 있다.

참고 : [https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/Testing](https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/Testing) (설명 잘 되어있음)