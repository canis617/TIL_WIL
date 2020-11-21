# GraphQL이란

- facebook에서 만든 Graph Query Language로 어플리케이션 레이어 쿼리 언어이다,

- [레퍼런스](https://graphql-kr.github.io/)

- API를 위한 쿼리 언어이며 타입 시스템을 사용하여 쿼리를 실행하는 서버사이드 런타임이다.

- GraphQL은 특정한 데이터베이스나 스토리지에 귀속되어 있지 ㅇ낳으며, 기존 코드와 데이터에 의해 대체된다.

- 기존에 사용하고 있는 REST API의 한계점을 극복하고자 나온 통신규약으로 REST API를 대체할 수 있다.

## REST API의 한계

- REST API란 모든 Resource들을 하나의 Endpoint에 연결하고 연결된 EndPoint는 Resource와 관련된 내용만 관리하게 하는 것
- 단순한 서비스에서는 아주 좋으나, 복잡한 서비스나 클라이언트의 요청사항에 따라 Over-Fetching과 Under-Fetching이 일어난다.
- 또한 REST API로 여러 환경에서 필요한 정보들을 Resource 별로 EndPoint를 가지게 구현하는 것이 어렵다.
- 한 마디로 비슷하지만 EndPoint가 다른 API가 많이 파생된다.

### Over-Fetching

- 예를 들어, 사용자의 데이터를 조회하는 /user/ API가 있다고 가정. 이 때, 사용자 번호:1 에 해당하는 데이터를 조회하면 다음과 같은 형태가 된다

```bash
response body
{
    "user_no": 1,
    "user_name": "test",
    "user_grade": "VIP",
    ...
}
```

- 여기서 클라이언트는 1번에 해당하는 유저의 이름만을 사용하고자 해도 유저 이름만 반환하는 API가 없다면 위와 같은 /user/1/ API를 호출한 다음, user_name을 가져와 사용해야 한다.
- 이 때 user_grade 등 필요없는 데이터도 같이 반환된다.
- 이러한 리소스의 낭비를 Over-Fetching이라고 부른다.

### Under-Fetching

- 쇼핑몰 서비스의 경우, 로그인한 사용자의 장바구니 정보를 보여준다고 가정하면 여러 API를 호출하게 된다.

```bash
/user/1/

/cart/

/notification/

/wish/
```

- 요청에 맞게 유효한 데이터를 보여주기 위해 여러 API를 호출하게 되는 경우를 Under-Fetching이라고 한다.

### REST API 목록

- REST API 방법론으로 API를 만든다고 하면 Resource 별로 EndPoint를 갖기 때문에 비슷하지만 다른 API를 생성하게 된다.

```bash
/item/

/item/detail/

/item/imgae/

/item/notice/

/item/manage/
```

- 이는 서비스가 커져갈 때 관리 포인트가 늘어나게 되므로 개발자나 클라이언트에게 부담이 됨

## GraphQL

- REST API의 한계를 극복하고자 나온 것.
- EndPoint는 통상 1개만 생성하고, 클라이언트에게 필요한 데이터는 클라이언트가 직접 쿼리를 작성, 호출하여 반환 받는다.

- Under-Fetching 문제를 GraphQL을 사용하여 해결

```js
// 요청 쿼리
query {
    user(user_no:1) {
        user_name
    }
}

// 반환 데이터
{
    "data": {
        "user": {
            "user_name": "jim",
        }
    }
}
```

- Over-Fetching 문제도 GraphQL을 사용해 해결

```js
// 요청 쿼리
query {
    cart {
        product_name
        price
    }
    notification {
        is_read
    }
    user(user_id: 1) {
        user_name
        user_grade
    }
}

// 반환 데이터
{
    "cart": [{
        "product_name": "shoes",
        "price": 12000
    }, ...],
    "notification": [{
        is_read:true
    }],
    "user": {
        "user_name": "jim",
        "user_grade": "VIP"
    }
}
```

### GraphQL의 장점

- 클라이언트가 필요한 데이터만 반환할 수 있다.
- 1번의 호출로 원하는 데이터를 한번에 가져올 수 있다.
  - REST API의 N+1 Problem을 해결 할 수 있다.
- 확장이 용이하다.

### GraphQL의 단점

- 백엔트, 클라이언트 개발자 양쪽 다 러닝 커브가 있다.
- 단순한 서비스에서는 사용하기가 복잡하다.
- 캐싱 기능의 구현이 복잡하다.
  - 대부분의 언어에서 라이브러리로 제공한다.
- 요청이 text로 날라가기 때문에 File 전송 등을 구현하기가 어렵다.

## 요약

- GraphQL은 REST API의 한계점을 해결하고자 나온 통신 규약.
- 항상 GraphQL이 REST API보다 낫다는 것이 아닌, 서비스 별로 잘 판단하여 레이어를 선택해야 한다.
