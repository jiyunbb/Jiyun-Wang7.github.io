---
author: Jiyun Wang
layout: post
title: 코드 리팩터링 2판, 6-8. 매개변수 객체 만들기
tags: [book, code refactoring]
---

```
하루에 한 챕터씩 읽고 정리해보기 도전!
```
## 6. 기본적인 리팩토링 
### 6-8. 변수 이름 바꾸기
- **배경**  
  - 데이터 항목 여러 개가 이 함수에서 저 함수로 함께 몰려다니는 경우, 데이터 구조를 하나로 모아주는게 좋다.
  - 데이터 뭉치를 데이터 구조로 묶으면 데이터 사이의 관계가 명확해진다는 이점을 얻는다. 게다가 함수가 이 데이터 구조를 받게 하면 매개변수 수가 줄어든다.
  - 모든 함수가 원소를 참조할 때 항상 똑같은 이름을 사용하기 때문에 일관성도 높여준다.
  - 이 리팩터링의 진정한 힘은 코드를 더 근본적으로 바꿔준다. -> 데이터 구조를 활용하는 형태로 프로그램 동작을 재구성 한다.
  - 데이터 구조에 담길 데이터에 공통으로 적용되는 동작을 추출해서 함수로 만드는 것이다.
  
- **절차**  
	- 적당한 데이터 구조가 아직 마련되어 있지 않다면 새로 만든다.
      - 클래스를 만드는 것이 좋다. 나중에 동작까지 함께 묶기 좋기 때문이다. 주로 데이터 구조를 값 객체로 만든다.
    - 테스트한다.
    - 함수 선언 바꾸기로 새 데이터 구조를 매개변수로 추가한다.
    - 테스트한다.
    - 함수 호출 시 새로운 데이터 구조 인스턴스를 넘기도록 수정한다. 하나씩 수정할 때마다 테스트한다. 
    - 기존 매개변수를 사용하던 코드를 새 데이터 구조의 원소를 사용하도록 바꾼다.
    - 다 바꿨다면 기존 매개변수를 제거하고 테스트한다.
 
- **진정한 값 객체로 거듭나기**
  - 매개변수 그룹을 객체로 교체하는 일은 진짜 값진 작업의 준비단계이다.
  - 클래스로 만들어두면 관련 동작들을 이 클래스로 옮길 수 있다는 이점이 생긴다.
  - 혹은 검사하는 메서드나 기타 다른 관련된 기능을 하는 메서드를 클래스에 추가할 수 있다.
  - 다른 유용한 동작도 범위 클래스로 옮겨서 코드베이스 저반에서 값을 활용하는 방식을 간소화 할 수 있다.
  - "동치성 검사 메서드" 를 추가해보자. (논리적 동치성 검사)
  
- **예시**

  ```python
  
  # step1. 아래처럼 구현. (인자값으로..)
  def is_acceptable_price(goods, min_price, max_price):
    return min_price < goods.price <= max_price
  
  
  # step2. 범위 인자값을 클래스로 구현
  class PriceRange:
    def __init__(self, min_price, max_price):
      self.min_price = min_price
      self.max_price = max_price
    
    def get_min_price(self):
      return self.min_price
  
    def get_max_price(self):
      return self.max_price
  
    
    # 추가로 값을 검증하는 함수를 추가하거나 다른 기능들을 확장해서 사용할 수 있다.
    def validate_price(self):
      if self.min_price < 0 or self.max_price < 0:
        return False
      
      if self.min_price >= self.max_price:
        return False
  
      if self.max_price > 1000000
        return False
      
      return True
  
  
  # 객체로 만들어서 넘긴다.
  price_range = PriceRange(min_price, max_price)
  def is_acceptable_price(goods, price_range):
    return price_range.min_price < goods.price <= price_range.max_price

  
  
  
  ```