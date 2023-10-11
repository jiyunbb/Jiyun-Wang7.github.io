---
author: Jiyun Wang
layout: post
title: 코드 리팩터링 2판, 6-5. 함수 선언 바꾸기
tags: [book]
---

```
하루에 한 챕터씩 읽고 정리해보기 도전!
```
## 6. 기본적인 리팩토링 
### 6.5 함수 선언 바꾸기 (= 함수 이름 바꾸기, 메서드명 변경, 매개변수 추가/제거)  
- **배경**  
  - 함수는 프로그램을 작은 부분으로 나누는 주된 수단이다. (연결부 역할)  
    - 함수의 이름이 아주 중요하다. 이름이 좋으면 함수의 구현 코드를 살펴볼 필요없이 호출문만 보고도 무슨일을 하는지 파악할 수 있다.  
    - 잘못된 함수를 발견하면 더 나은 이름으로 즉시 바꿔본다. -> 나중에 그 코드를 다시 볼 때 무슨 일을 하는지 "또" 고민하지 않도록!  
    - 좋은 이름을 떠올리는 데 효과적인 방법 -> 함수의 목적을 설명하는 주석을 적어본다. 주석을 작성하면서 걸맞는 이름이 떠오를 가능성이 높다.  
  - 매개변수는 함수가 외부 세계와 어우러지는 방식을 정의한다.  
    - 매개변수를 어떤 값을 받느냐에 따라 의존성을 제거하여 활용 범위가 넓어질 수 있고, 다른 모듈과의 결합을 제거할 수 있다.  
    - 함수의 캡슐화 수준을 높이는데 매개변수가 중요한 역할을 한다.  
  
- **간단한 절차**  
	- 매개변수 제거 시 먼저 함수 본문에서 제거 대상 매개변수를 참조하는 곳이 없는지 확인한다.  
	- 메서드 선언을 원하는 형태로 바꾼다.  
	- 기존 메서드 선언을 참조하는 부분을 모두 찾아서 바뀐 형태로 수정한다.  
	- 테스트한다.  
	- 혹시, 이름 변경과 매개변수 추가를 모두 하고 싶다면 하나씩 독립적으로 수행하자.  
  
  
- **예시 1. 함수 이름 바꾸기(간단한 절차)**
	- 함수 선언을 수정하고, total_price()을 호출한 곳을 get_total_price()로 변경하고, total_price() 함수를 제거한다.  
	- 매개변수 추가나 삭제도 똑같이 수행한다.  
	- 단점 : 호출문과 선언문을 한번에 수정해야 한다. (다형성으로 한 경우도 더 복잡..)  
 
	```python  
	# 수정 전  
	def total_price(goods, quantity):  
		return goods.price * quantity 
	``` 

	```python  
	# 수정 후  
	def get_total_price(goods, quantity):  
		return goods.price * quantity 
	```

  
- **마이그레이션 절차 (간단한 절차로 해결이 어려울 때)**  
	- 함수의 본문을 적절히 리팩터링한다.  
	- 함수 본문을 새로운 함수로 추출한다. (임시 이름을 사용해서라도..)  
	- 추출한 함수에 매개변수를 추가해야한다면 "간단한 절차"를 밟는다.  
	- 테스트한다.  
	- 기존 함수를 인라인한다.  
	- 이름을 임시로 붙여뒀다면 함수 선언 바꾸기를 한 번 더 적용해서 원래 이름으로 되돌린다.  
	- 테스트한다.  
  
- **예시 1. 함수 이름 바꾸기(마이그레이션 절차)**
	- 본문 전체를 새로운 함수로 추출한다.
	- 수정한 코드를 테스트한 뒤 예전함수로 인라인한다.
	- 예전 함수를 호출하는 부분을 모두 새 함수로 바꾼다.  
	- 기존 함수 삭제한다.
	 	
	```python  
	def total_price(goods, quantity):  
		# 2. 예전 함수를 인라인한다.   
		return get_total_price(goods, quantity)  

	def get_total_price(goods, quantity):
		# 1. 본문 전체를 새로운 함수로 추출한다.  
		return goods.price * quantity    
	```  

	```python  
	# 예전 함수를 호출하는 부분을 모두 새 함수로 바꾼다.
	# 기존 함수 삭제한다.
	def get_total_price(goods, quantity):  
		return goods.price * quantity 
	```
	

- **예시 2. 매개변수 추가하기(마이그레이션 절차)**  
	- 본문 전체를 새로운 함수로 추출한다. (기억이 나지 않으면 임시 이름으로)
	- 새 함수의 선언문과 호출문에 원하는 매개변수를 추가한다.  
	- assertion을 추가하여 검증한다.
	- 이제 기존 함수를 인라인하여 호출 코드들이 새 함수를 이용하도록 고친다.  
	- 새 함수의 이름을 기존 함수 이름으로 고치고 기존 함수를 제거한다.  
	-  temp_get_total_price 를 이후에 get_total_price 로 네이밍을 변경한다.


	```python  
	def get_total_price(goods, quantity):  
		return temp_get_total_price(goods, quantity)

	def temp_get_total_price(goods, quantity):  
		# 본문 전체를 새로운 함수로 추출한다.
		return goods.price * quantity 

	def get_total_price(goods, quantity):  
		return temp_get_total_price(goods, quantity)     

	def temp_get_total_price(goods, quantity, discount_amount):  
		# 2. 새 함수의 선언문과 호출문에 원하는 매개변수 discount_amount 추가한다.  
		# 3. assertion을 추가하여 검증한다.  
		assert discount_amount >= 0 
		return goods.price * quantity - discount_amount 

	def get_total_price(goods, quantity):  
		# 2. 예전 함수를 인라인한다.   
		return temp_get_total_price(goods, quantity, 0)  

	def temp_get_total_price(goods, quantity, discount_amount):  
		# 1. 본문 전체를 새로운 함수로 추출한다.  
		# 2. 새 함수의 선언문과 호출문에 원하는 매개변수 discount_amount 추가한다.  
		# 3. assertion을 추가하여 검증한다.  
		assert discount_amount >= 0 
		return goods.price * quantity - discount_amount 
	``` 
  
- **예시 3. 매개변수를 속성으로 바꾸기(마이그레이션 절차)**
	- 함수 선언을 바꿀 때 함수 추출을 한다. (본문을 살짝 리팩터링해두면 작업이 수월)
	- 함수 추출하기로 새 함수 만든다.  
	- 기존 함수 안에 변수로 추출해둔 입력 매개변수를 인라인한다. (변수 인라인 기법)  
	- 함수 인라인하기로 기존 함수의 본문을 호출문들에 집어넣는다. (기존 함수 호출문 <-> 새 함수 호출문 교체)  
	- 함수 선언 바꾸기를 다시 한번 적용하여 새 함수의 이름을 기존 함수 이름으로 변경한다.

	```python 
	# 수정 전
	def is_seoul_tel_number(user): 
		# 유저번호가 서울인지 체크
		return "02" in user.tel_number

	def is_lived_in_seoul(user):
		return is_seoul_tel_number(user)

	# 수정 후 
	def is_seoul_tel_number(tel_number): 
		# 기업번호나 유저번호 체크 모두 범용적으로 사용할 수 있게 됨.
		return "02" in user.tel_number

	def is_lived_in_seoul(user):
		# 전화 번호만 넘기는 것으로 수정.
		return is_seoul_tel_number(user.tel_number)
	``` 


