---
author: Jiyun Wang
layout: post
title: 코드 리팩터링 2판, 6-3. 변수 추출하기
tags: [book]
---

```
하루에 한 챕터씩 읽고 정리해보기 도전!
```

## 6. 기본적인 리팩토링 
### 6.3 변수 추출하기 (반대 : 변수 인라인하기)
- **배경**
	- 표현식이 너무 복잡해서 이해하기 어려울 때, 지역변수를 활용하면 표현식을 쪼개서 관리를 더 쉽게 만든다.
	- 추가한 변수는 디버깅에 도움이 된다. 
		- breakpoint로 찍어서 값을 확인하기 용이함.
		- 상태를 출력하는 문장을 추가할 수 있음.
	- 현재 함수 안에서만 의미가 있다면 변수로 추출하는 것이 좋다. 다만 클래스 전체에 영향을 주거나, 그 이상의 범위까지 영향을 준다면 변수 추출이 아니라 함수 추출이 더 적절하다. (함수로 추출해야 여기저기 사용하면서 표현식 중복을 막을 수 있으니까)

- **절차**
	- 추출하려는 표현식을 수정할 때 부작용이 없을지 확인한다.
	- 불변 변수를 하나 선언하고 변수화시킬 대상 표현식에 복제본을 대입한다.
	- 원본 표현식 코드를 위에서 선언한 불변 변수로 교체한다.
	- 테스트한다.
	- 표현식이 한 함수 내 여러 곳에서 사용된다면 각각 새로 만든 변수로 교체한다. 테스트한다. 반복.

- **예시 1. 간단한 케이스**
	- 한 함수 내에서만 영향을 주는 경우, 단순히 변수 생성, 표현식 대입, 변수로 교체, 테스트를 반복한다.
	```python
	# 1단계 -> 복잡한 표현식이 섞인 값을 리턴하는 함수 정의
		def get_price(order):
			return order.quantity * order.item_price -
				max(0, order.quatity - 500) * order.item_price * 0.05 +
				min(order.quantity * order.item_price * 0.1, 100)
	```
	```python
	# 2단계 -> 변수 추출하기를 이용하여 하나씩 뽑아낸다.
		def get_price(order):
			base_price = order.quantity * order.item_price
			quatity_discount = max(0, order.quatity - 500) * order.item_price * 0.05
			shipping = min(order.quantity * order.item_price * 0.1, 100)
			
			return base_price - quatity_discount + shipping
	```
	
- **예시 2. 클래스 안에서 (영향범위가 함수 밖)**
	- 영향 범위가 함수 밖(여기에서는 클래스)인 경우에는 변수가 아니라 함수로 추출하는 편이 더 낫다. (객체 장점을 살릴수도 있고..)
	```python
	# 복잡한 표현식이 섞인 값을 리턴하는 함수 정의
		class OrderCalculator:
			def __init__(self, order):
				self.order = order
			
			def get_quatity(self):
				return self.order.quantity
				
			def get_quatity(self):
				return self.order.quantity
			
			def get_final_price(self):
				return self.order.quantity * order.item_price -
					max(0, self.order.quatity - 500) * self.order.item_price * 0.05 +
					min(self.order.quantity * self.order.item_price * 0.1, 100)
	```
	```python
	# 클래스 전체적으로 영향을 줄 수 있는 경우, 혹은 객체사용의 이점을 더 잘 살릴 수 있는 경우를 고려하여 함수로 추출했다.
		class OrderCalculator:
			def __init__(self, order):
				self.order = order
			
			def get_quatity(self):
				return self.order.quantity
				
			def get_quatity(self):
				return self.order.quantity
			
			# 새로 추가된 함수 3개. (클래스 외부에서도 사용할 수 있다.)
			def get_base_price(self):
				return self.order.quantity * order.item_price
			
			def get_quatity_discount(self):
				return max(0, self.order.quatity - 500) * self.order.item_price * 0.05
			
			def get_shipping(self):
				return min(self.order.quantity * self.order.item_price * 0.1, 100)
			
			def get_final_price(self):
				return self.get_base_price() - self.get_quatity_discount() + self.get_shipping()
	```

- **결론**
	- 객체의 장점을 활용할 수 있는 형태로 리팩토링이 가능하다. 객체는 특정 로직과 데이터를 외부와 공유하려 할 때 공유할 정보를 설명해주는 적당한 크기의 문맥이 되어준다.
	- 덩치가 큰 클래스에서 공통 동작을 별도의 이름으로 뽑아내서 추상화해두면 그 객체를 다룰 때 쉽게 활용할 수 있어서 유용하다.
