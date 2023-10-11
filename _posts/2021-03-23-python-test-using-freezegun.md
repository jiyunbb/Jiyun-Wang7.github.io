---
author: Jiyun Wang
layout: post
title: 시간 의존적인 테스트 코드 작성 시 freezegun 활용
tags: [python, test]
---

### 찾아보거나 알게된 배경

---

이전에 사용해본 적이 있고, 마케팅 수신 동의 메일 발송 시에 유저를 뽑을 때 시간에 의존적인 테스트 코드를 짜야하는데, freezegun을 사용하여, 항상 동일한 테스트 결과를 낼 수 있다. (이번에 풀리퀘스트에 올라갈 예정)

### 요약

---

- 특정 시점에서의 테스트가 필요한 경우 freezegun 을 사용해서 현재 시간값을 고정한다.
- @freeze_time 데코레이터를 이용하는 것이고, 데코레이터에 적용된 시간이 '현재시간' 이라고 이해하면 된다.
- 시간에 의존적인 테스트를 할 때 쓰면 유용하다.
- 정확하게는 django.util.timezone.now()가 freeze_time으로 세팅한 시간으로 설정된다.
- 따라서 freeze_time을 데코레이터로 걸고 테스트를 통해 함수나 api를 호출한 뒤, timezone.now()를 찍어보면 내가 임의로 지정한 날짜시간으로 노출됨을 알 수 있다.

아래 예시 참조!
```python
@freeze_time('2020-03-18')
@mock.patch('user.cron.EmailClient.send_email', MockEmailClient.send_email)
@mock.patch('user.cron.EmailClient.send_email_with_retry', MockEmailClient.send_email_with_retry)
def test_simple(self):
    result_dict = marketing_terms_agreement_send_email()
    self.assertGreaterEqual(int(result_dict['success_count']), 1)
    self.assertEqual(FirstTable.objects.get(user=self.user).send_status, 1)
    self.assertEqual(SecondTable.objects.get(user=self.user).modified_at, timezone.now())
```


### 참고자료

---

[https://pypi.org/project/freezegun/](https://pypi.org/project/freezegun/)

[https://8percent.github.io/2017-05-31/test-guide/](https://8percent.github.io/2017-05-31/test-guide/)