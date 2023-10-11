---
author: Jiyun Wang
layout: post
title: REST API 디자인 규칙 - 4장
tags: [book]
---

### 4.1 HTTP 헤더
- HTTP는 표준 헤더 집합을 정의하고 그 중 일부는 요청된 리소스 관련 정보를 제공한다. 그리고 몇몇 헤더는 메시지에 전달되는 표현 관련 정보를 나타내며, 중간 캐시를 조절하는 지시자 역할을 하기도 한다.
- **규칙**
    - Content-Type을 사용해야 한다.
        - 요청이나 응답 메시지 바디에 있는 데이터 타입
    - Content-Length를 사용해야 한다.
        - 바이트 단위로 바디의 크기
        - 클라는 이 값을 이용하여 바이트의 크기를 올바로 읽었는지 알 수 있고, HEAD 요청을 사용하여 데이터를 다운로드 하지 않고도 바디가 얼마나 큰지 알 수 있음.
    - Last-Modified
        - 응답 메시지에만 사용한다. 이 응답 헤더의 값은 타임스탬프로 리소스 표현 상태 값이 바뀐 마지막 시간을 의미함.
        - HTTP 헤더에 서버가 알고있는 가잦ㅇ 마지막 수정된 날짜와 시각을 담고 있고, 저장된 리소스가 이전과 같은지 유효성 검사자로 사용됨. Etag 헤더보다는 덜 정확하지만 대비책으로 사용됨.
        - 조건부 요청이 가능함. (`[If-Modified-Since](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/If-Modified-Since)` 또는 `[If-Unmodified-Since](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Unmodified-Since)` )
    - Etag(Entity Tag)는 응답에 사용해야 한다.
        - Etag 값은 응답 엔티티에 포함된 표현 상태의 특정 버전을 나타내는 일련의 문자열
        - 웹 캐시 유효성 검증에 사용되며, 리소스 특정 버전에 대한 고유값임. 리소스 내용이 업데이트 되면 Etag도 바뀜.
        - 엔티티 태그 값이 바뀌지 않았다면, 304-NOT MODIFIED.
    - 스토어는 조건부 PUT 요청을 지원해야 한다.
        - PUT을 통해 리소스를 새로 추가하거나 갱신할 수 있음. 하지만, 같은 PUT으로 요청하였지만 처리가 다르게 되어야하는데 이를 어떻게 구분해야할까? → 헤더를 통해 모호함을 해결할 수 있음.
        - 조건부 요청인 `[If-Modified-Since](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/If-Modified-Since)` 또는 `[If-Unmodified-Since](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Unmodified-Since)`  요청 헤더를 통해서 클라이언트의 의도를 알 수 있음.
            - `[If-Modified-Since](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/If-Modified-Since)` : 리소스의 상태 표현이 헤더 값에 표현된 타임스탬프 시간 이후로 바뀌지 않았을 경우에만 동작
            - `[If-Unmodified-Since](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Unmodified-Since)`  : Entity Tag(Etag) 값임. 헤더에 제공된 엔티티 태그 값과 REST API에 의해 저장되거나 계산된 표현 상태의 현재 태그 값의 정확한 비교에 기초하여 동작.
            - 예시
                - c1이 /objects/2113 으로 식별되는 새로운 데이터를 저장하기 위해 PUT 요청 보냄
                    - REST API에서는 이 URI는 스토어에 저장되어 있지 않기 때문에 데이터를 삽입하고 201 응답 보냄
                - c2가 /objects/2113 을 PUT으로 요청함.
                    - REST API에서는 URI가 이미 있는 리소스라는 것을 알지만, 클라이언트 의도가 생성인지 갱신인지 알 수 없음. c1에 의해 생성된 데이터를 덮어쓸 것인지 결정하는 충분한 정보를 REST API가 알지 못하기 떄문임. 그래서 409 응답 보냄.
                    - c2가 저장된 데이터를 갱신하고자 할 땐 `If-Matched` 헤더를 포함해서 재요청 할 수 있음. 응답에 리소스 사태의 갱신된 표현이 응답에 포함되어 있으면, API는 Last-Modified와 Etag 헤더 값을 포함해서 갱신여부를 반영함.
        - Location은 새로 생성된 리소스의 URI를 나타내는 데 사용해야 한다.
            - 클라이언트의 관심 범위에 있는 리소스를 식별하는 URI, 컬렉션이나 스토어에 성공적으로 리소스를 생성했다는 응답
        - Cache-Control, Expires, Date 응답 헤더는 캐시사용을 권장하는데 사용해야 한다.
            - Cache-Control 헤더와 초 단위의 max-age 값이 같은 갱신주기를 함께 사용해야 한다.
        - Cache-Control, Expires, Pragma 응답 헤더는 캐시사용을 중지하는데 사용해야 한다.
            - no-cache, no-store로 설정
        - 캐시 기능은 사용해야 한다.
        - 만기 캐싱 헤더는 200 응답에 사용해야 한다.
        - 만기 캐싱 헤더는 3xx와 4xx 응답에 선택적으로 사용될 수 있다.
        - 커스텀 HTTP 헤더는 HTTP 메서드의 행동을 바꾸는데 사용해서는 안된다.
            - 커스텀 헤더는 정보 전달이 목적일 때만 선택적으로 사용할 수 있다.

### 4-2. 미디어 타입
- 요청이나 응답의 메시지 바디 안에 있는 데이터 형태를 식별하기 위해 Content-type 헤더 값을 미디어 타입이라고 함.
- **문법**
    - `type “/” subtype ; (세미콜론 구분자)`
- **종류**
    - text/plain, text/html, image/jpeg, application/xml, application/atom+xml, application/javascript, application/json
- **벤더 고유 미디어 타입**
    - 접두어 vnd 가 붙으며 특정 업체에서 소유 및 관리
        - 예시 : `application/vnd.ms-excel`
- **규칙**
    - 애플리케이션 고유 미디어 타입을 사용해야 한다.
- **미디어 타입 스키마 버저닝**
    - **예시** : `http://api.ssss.sssss/soccer/Player-2`
    - **규칙**
        - 리소스의 표현이 여러 가지 가능할 경우 미디어 타입 협상을 지원해야 한다.
            - `Accept` 태그
        - query 변수를 사용한 미디어 타입 선택을 지원할 수 있다.
            - 예시 : `GET /bookmarks/mikemassedotcom?accept=application/xml`
