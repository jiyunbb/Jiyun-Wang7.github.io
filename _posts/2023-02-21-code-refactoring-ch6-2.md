---
author: Jiyun Wang
layout: post
title: 코드 리팩터링 2판, 6-2. 함수 인라인하기
tags: [book]
---

```
하루에 한 챕터씩 읽고 정리해보기 도전!
```
## 6. 기본적인 리팩토링 
### 6.2 함수 인라인하기 (반대 : 함수 추출하기)
- **배경**
	- 함수 본문을 이름만큼 깔끔하게 리팩터링할 때, 그 함수를 제거할 수 있다.
	- 간접 호출은 유용할 수도 있지만 쓸데없는 간접 호출은 가독성을 해친다. (매우 거슬림.)
	- 리팩터링 과정에서 "잘못" 추출된 함수(함수 추출하기 기법을 사용하여)들을 다시 인라인한다. 즉, 추출된 함수들을 원래 함수(코드)로 합친다음 필요하다면 원하는 형태로 다시 추출하는 것이다.
	- 그렇다면 어떤 것들이 인라인 대상일까?
		- 간접 호출을 너무 과하게 쓰는 코드들.
		- 단순히 다른 함수로 위임하기만 하는 함수들이 너무 많은 경우, 얽혀있는 관계들이 복잡할 것이다. 그 때 인라인을 해버린다.

- **절차**
	- 다형 메서드인지 확인한다.
		- 서브클래스에서 오버라이드하는 메서드는 인라인하면 안된다.
	- 인라인할 함수를 호출하는 곳을 모두 찾는다.
	- 각 호출문을 함수 본문으로 교체한다. (본문 코드로 합친다.)
	- 하나씩 교체할 때마다 테스트 한다.
		- 하나씩 처리할 때 마다 확인이 필요하다. 까다로운 부분이 있다면 남겨두고 여유를 가지고 처리한다.
	- 함수 정의(원래 함수)를 삭제한다.

- **예시 1. 간단한 케이스**
	- 호출되는 함수의 반환문을 그대로 복사해서 호출하는 함수의 호출문을 덮어쓰면 인라인이 끝이난다.
	```python
	# 1단계 -> 간접 호출로 구성되어 있음.
		def get_review_rating_by_count_condition(review):
			# 갯수 조건에 따라 review rating을 보여줄지 0을 보여줄지 결정하는 함수
			review.rating if is_review_count_more_than_five(review) else 0
		
		def is_review_count_more_than_five(review):
			# 리뷰 갯수가 5개 초과인지 확인하는 함수
			return review.count > 5
	```
	```python
	# 2단계 -> 인라인을 통해 통폐합.
		def get_review_rating_by_count_condition(review):
			# 갯수 조건에 따라 review rating을 보여줄지 0을 보여줄지 결정하는 함수
			review.rating if review.count > 5 else 0
	```
	
- **예시 2. 수정이 조금 더 필요한 케이스**
	- 서로 공유하고 있는 인자값의 네이밍이 다른 경우, 해당 예시에서는 review와 survey는 같은 정보이지만 네이밍이 다르다.
	```python
	# 같은 정보를 공유하고 있지만 인자값의 네이밍이 다른 경우 인라인시에 적절하게 수정해주어야 한다.
		def get_total_review_rating(review_list):
			total_review_rating = 0
			for review in review_list:
				total_review_rating += review.rating
				print_survey_info(review)
			
			return total_review_rating

		def print_survey_info(survey):
			print("id", survey.id)
			print("product", survey.product_id)
			print("user", survey.user_id)
	```
	```python
		# 여러 문장을 호출한 곳으로 옮기기 기법을 사용하여 잘라내서, 붙이고, 다듬는 방식으로 수정한다.
		def get_total_review_rating(review_list):
			total_review_rating = 0
			for review in review_list:
				total_review_rating += review.rating
				
				# survey 라는 워딩은 review로 바꿔줌.
				print("id", review.id)
				print("product", review.product_id)
				print("user", review.user_id)
				
			return total_review_rating
	```

- **결론**
	- 항상 단계를 잘게 나눠서 처리하는 것이 중요하다. 일단 기존에 함수를 작게 만들어두었다면 인라인을 단번에 처리할 수 있을 수 있다.
		- 문장을 호출한 곳으로 옮기기 기법을 사용하여 작업을 잘게 나누어 단계적으로 처리한다.
