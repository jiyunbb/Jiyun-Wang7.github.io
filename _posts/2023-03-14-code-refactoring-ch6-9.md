---
author: Jiyun Wang
layout: post
title: 코드 리팩터링 2판, 6-9. 여러 함수를 클래스로 묶기
tags: [book]
---

```
하루에 한 챕터씩 읽고 정리해보기 도전!
```
## 6. 기본적인 리팩토링 
### 6-9. 여러 함수를 클래스로 묶기
- **배경**  
  - 클래스는 기본적인 빌딩 블록이다.
  - 클래스는 데이터와 함수를 하나의 공유 환경으로 묶은 후, 다른 프로그램 요소와 어우러질 수 있도록 그중 일부를 외부에 제공한다.
  - 클래스는 객체지향 언어의 기본인 동시에 다른 패러다임 언어에도 유용하다.
  - 공통 데이터를 중심으로 긴밀하게 엮어 작동하는 함수 무리를 발견한다면 클래스로 묶어보자.
    - 이 함수들이 공유하는 공통 환경을 더 명확하게 표현할 수 있고, 각 함수에 전달되는 인수를 줄여서 객체 안에서의 함수 호출을 간결하게 만들 수 있다.
  - 이 리팩터링은 이미 만들어진 함수들을 재구성할 때는 물론, 새로 만든 클래스와 관련하여 놓친 연산을 찾아서 새클래스의 메서드로 뽑아내는데도 좋다.
    - 여러 함수를 변환 함수로 묶기 방법도 있음.
  - 클래스로 묶을 때의 장점은 클라이언트가 객체의 핵심 데이터를 변경할 수 있고, 파생 객체들을 일관되게 관리할 수 있다는 점이다.
  
- **절차**  
	- 함수들이 공유하는 공통 데이터 레코드를 캡슐화 한다.
      - 공통 데이터가 레코드 구조로 묶여 있지 않다면 먼저 매개변수 객체 만들기로 데이터를 하나로 묶는 레코드를 만든다.
    - 공통 레코드를 사용하는 함수 각각을 새 클래스로 옮긴다.
      - 공통 레코드의 멤버는 함수 호출문의 인수 목록에서 제거한다.
    - 데이터를 조작하는 로직들은 함수로 추출해서 새 클래스로 옮긴다.
  
- **예시**
  ```python
    
    # 유저 1
    user1_calculated_goods_price = (goods.price - goods.discount_price) * goods.amount
    
    # 유저 2
    user2_calculated_goods_price = (goods.price - goods.discount_price) * goods.amount
    
    # 유저 3
    user3_calculated_goods_price = (goods.price - goods.discount_price) * goods.amount
    
    # step1. 캡슐화 및 함수 옮기기
    class goods:
      def __init__(self, price, vat, discount_rate):
        self.price = price
        self.amount = amount
        self.discount_price = discount_price
      
      def get_price():
        return self.price
        
      def get_amount():
        return self.amount
  
      def get_discount_price():
        return self.discount_price
      
      # 공통 수식 함수화 및 클래스 이동
      def calculate_total_price(self):
        return (self.price - self.discount_price) * self.amount
  
    
    # step 2. 함수 대체
  
    # 유저 1
    user1_calculated_goods_price = goods.calculate_total_price()
    
    # 유저 2
    user2_calculated_goods_price = goods.calculate_total_price()
    
    # 유저 3
    user3_calculated_goods_price = goods.calculate_total_price()
  ```