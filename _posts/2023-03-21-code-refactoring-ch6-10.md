---
author: Jiyun Wang
layout: post
title: 코드 리팩터링 2판, 6-10. 여러 함수를 변환 함수로 묶기
tags: [book, code refactoring]
---

```
하루에 한 챕터씩 읽고 정리해보기 도전!
```
## 6. 기본적인 리팩토링 
### 6-10. 여러 함수를 변환 함수로 묶기
- **배경**  
  - 변환 함수는 원본 데이터를 입력받아서 필요한 정보를 모두 도출한 뒤, 각각을 출력 데이터의 필드에 넣어 반환한다. 이렇게 해두면 도출 과정을 검토할 일이 생겼을 때 변환 함수만 살펴보면 된다.
  - 이 리팩터링 대신 여러 함수를 클래스로 묶기로 처리해도 된다.
  - 원본 데이터가 코드 안에서 갱신될 때는 클래스로 묶는 편이 훨씬 낫다. 변환 함수로 묶으면 가공한 데이터를 새로운 레코드에 저장하므로, 원본 데이터가 수정되면 일관성이 깨질 수 있기 때문이다.
  - 여러 함수를 한데 묶는 이유 하나는 도출 로직이 중복되는 것을 피하기 위해서다.
    - 중복 코드는 나중에 로직을 수정할 때 골치를 썩인다. (반드시 수정할 일이 생긴다.) 중복 코드라면 함수 추출하기로 처리할 수도 있지만, 추출한 함수들이 프로그램 곳곳에 흩어져서 나중에 프로그래머가 그런 함수가 있는지조차 모르게 될 가능성이 있다.
  - 변환함수 -> 변환 단계에서 미가공 측정값을 입력받아서 다양한 가공 정보를 덧붙여 반환하는 것이다.
  
- **절차**  
	- 변환할 레코드를 입력받아서 값을 그대로 반환하는 변환 함수를 만든다.
      - 깊은 복사로 처리해야 한다. 변환 함수가 원본 레코드를 바꾸지 않는지 검사하는 테스트를 마련해두면 도움될 때가 많다.
    - 묶을 함수 중 함수 하나를 골라서 본문 코드를 변환 함수로 옮기고, 처리 결과를 레코드에 새 필드로 기록한다. 그런 다음 클라이언트 코드가 이 필드를 사용하도록 수정한다.
      - 로직이 복잡하면 함수 추출하기 부터 한다.
    - 테스트한다.
    - 나머지 관련 함수도 위 과정에 따라 처리한다.
  
- **예시**
  ```python

    # 유저 1
    a_reading = acquire_reading()
    base = (base_rate(a_reading.month, a_reading.year) * a_reading.quantity)
    taxable_charge = Math.max(0, base - tax_threshold(a_reading.year)
    
    # 이미 base에 대한 함수가 중복으로 존재하고 있음.
    def calculate_base_charge(a_reading): 
      return base_rate(a_reading.month, a_reading.year) * a_reading.quantity
    
    # step1. 입력 객체를 그대로 복사해 봔환하는 변환 함수를 만든다. 
    # 변환함수가 될 함수
    def enrich_reading(original):
      result = copy.deepcopy(original)
      return result
  
    # step2. 변경하려는 계산 로직 중 하나 고른다. 이 계산 로직에 측정값을 전달하기 전에 부가 정보를 덧붙이도록 수정한다.
    # 유저 1
    a_reading = acquire_reading()
    base = enrich_reading(a_reading) # 이거 변경
    taxable_charge = Math.max(0, base - tax_threshold(a_reading.year)
  
    def enrich_reading(original):
      result = copy.deepcopy(original)
      result.base_charge = calculate_base_charge(result)
      return result
    
  
    # 결과!
    a_reading = enrich_reading(a_reading)
    base = a_reading.base_charge
    taxable_charge = a_reading.taxable_charge
  
    def enrich_reading(original):
      result = copy.deepcopy(original)
      result.base_charge = calculate_base_charge(result)
      result.taxable_charge = Math.max(0, result.base_charge - tax_threshold(result.year)
      return result
  ```