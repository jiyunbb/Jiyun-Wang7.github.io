---
author: Jiyun Wang
layout: post
title: REST API 디자인 규칙 - 2장
tags: [book, rest api design principal]
---

### 2.1 URI
- REST API 설계자는 URI를 만들 때 부터 REST API 리소스 모델을 클라이언트에 전달할 수 있어야 한다.

### 2.2 URI 형태
- **규칙: 슬래시 구분자는 계층관계를 나타내는데 사용한다.**
    - 리소스간 계층관계를 표현한다.
- **규칙 : URI 마지막 문자로 슬래시를 포함하지 않는다.**
    - 혼란을 초래할 수 있다.
    - 클라이언트에서 끝에 슬래시를 붙이는 경우 보통 슬래시가 없는 URI로 전달한다. (http status code. 301)
- **규칙 : 하이픈은 URI 가독성을 높이는데 사용한다.**
    - 문장 내 공백을 하이픈으로 쓸 수 있다.
- **규칙 : 밑줄은 URI에 사용하지 않는다.**
    - 밑줄은 보기 어렵거나 다른 밑줄에 겹쳐서 문자가 완전히 가려질 수 있어서 이러한 문제를 회피하기 위해 밑줄은 사용하지 않는다. 대신 하이픈 사용한다.
- **규칙 : URI 경로에는 소문자가 적합하다.**
    - 스키마와 호스트를 제외하고는 일관적으로 소문자로 작성한다. 혼란을 야기할 수 있다.
    - 리소스 명에서 소문자와 대문자는 다르게 취급이 된다.
- **규칙 : 파일 확장자는 URI에 포함시키지 않는다.**
    - 미디어 타입에 의존한다. 파일 확장자를 강조하고 싶다면 대신 Content-Type 헤더를 통해 컨텐츠를 처리하는 방법을 결정하는 것이 좋다.

### 2.3 URI 권한 설계
- **규칙 : API에 있어서 서브 도메인은 일관성 있게 사용해야 한다.**
    - API 전체 도메인 이름에 일관적인 서브 도메인 이름을 지정한다
    - e.g., http://api.hwahae.co.kr
- **규칙 : 클라이언트 개발자 포탈 서브 도메인 이름은 일관성 있게 만들어야 한다.**
    - 일반적으로 서브 도메인명이 developer 이다.
    - e.g., http://developer.hwahae.co.kr

### 2.4 리소스 모델링
- 리소스 모델링은 API의 주요 개념을 확실하게 잡는 훈련과 같다.
- 경로 설계하기 전 리소스 모델을 충분히 생각해보고 결정하는 것이 좋다.

### 2.5 리소스 원형
- **도큐먼트**
    - 객체 인스턴스나 데이터베이스 레코드와 유사한 단일 개념이다.
    - e.g.,
        - http://api.hwahae.co.kr/users/{id}
        - http://api.hwahae.co.kr/countries/korea/cities/seoul
- **컬렉션**
    - 서버에서 관리하는 디렉터리이다.
    - e.g.,
        - http://api.hwahae.co.kr/users
        - http://api.hwahae.co.kr/countries
        - http://api.hwahae.co.kr/countries/korea/cities
- **스토어**
    - 클라이언트에서 관리하는 리소스 저장소이다.
    - API 클라이언트가 리소스를 넣고, 꺼내고, 삭제할 시기를 결정할 수 있다.
    - HTTP Method CRUD 표현
    - e.g.,
        - http://api.hwahae.co.kr/users/{id}/playlists
- **컨트롤러**
    - REST API의 CRUD와는 논리적으로 매핑되지 않는 고유의 행동을 컨트롤러 리소스를 이용하여 수행한다.
    - e.g.,
        - http://api.hwahae.co.kr/push/re-send → 뭐 푸시를 다시 보낸다 이런정도?

### 2.6 URI 경로 디자인
- URI 경로마다 각 부분에 의미있는 값들을 줌으로써 명확하게 표현할 수 있다.
- **규칙 : 도큐먼트 이름으로는 단수명사를 사용한다.**
    - e.g.,
        - http://api.hwahae.co.kr/teams/backend/developers/jiyun
            - 팀들 > 백엔드 > 개발자들 > 지윤
- **규칙 : 컬렉션 이름으로는 복수명사를 사용한다.**
    - e.g.,
        - http://api.hwahae.co.kr/teams
        - http://api.hwahae.co.kr/teams/backend/developers
            - 팀들 > 백엔드 > 개발자들
- **규칙 : 스토어 이름으로는 복수명사를 사용한다.**
    - e.g.,
        - http://api.hwahae.co.kr/teams/backend/developers/jiyun/hobbies
            - 팀들 > 백엔드 > 개발자들 > 지윤 > 취미들
- **규칙 : 컨트롤러 이름으로는 동사나 동사구를 사용한다.**
    - e.g.,
        - http://api.hwahae.co.kr/dbs/reindex
            - 디비들을 재인덱스한다 라는 동사표현. (CRUD로 표현할수 없음)
- **규칙 : 경로 부분 중 변하는 부분은 유일한 값으로 대체한다.**
    - 어떤 경로 부분은 변수처럼 변환하여 유일한 식별자로 채운다.
    - e.g.,
        - http://api.hwahae.co.kr/users/{user_id}/hobbies/{hobby_id}
            - 특정 유저의 특정 취미를 의미한다.
- **규칙 : CRUD 기능을 나타내는 것은 URI에 사용하지 않는다.**
    - HTTP 메소드를 이용하여 표현한다
    - GET, POST, PUT, DELETE

### 2.7 URI Query 디자인
- 클라이언트 검색이나 필터링 같은 추가적인 상호작용 능력을 제공 (옵셔널)
- **규칙 : URI 쿼리 부분으로 컬렉션이나 스토어를 필터링 할 수 있다.**
    - e.g.,
        - /users?gender=female
- **규칙 : URI 쿼리는 컬렉션이나 스토어의 결과를 페이지로 구분하여 나타내는데 사용한다.**
    - 쿼리 구성요소를 사용하여 컬렉션이나 스토어의 결과를 pageSize, pageStartIndex 등 파라미터값으로 페이지화 한다.
