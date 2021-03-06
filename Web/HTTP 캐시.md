# 캐시란

- [참고자료](https://pjh3749.tistory.com/264)

- 이전에 가져온 리소스들을 재사용함으로써 현저하게 성능을 향상시킬 수 있다.

- 웹 캐시는 레이턴시와 네트워크 트래픽을 줄여주므로 리소스의 표현에 필요로 하는 시간을 줄여준다.

- 모든 브라우저는 HTTP 캐시를 구현하고 있다!

- 우리가 할 일은 각 서버단에서 맞는 HTTP 헤더를 내려줘 브라우저에게 응답 캐시를 언제 얼마나 보유할지 가이드하는것.

![캐시 이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlEP4Y%2FbtqyBza3dQW%2F2mkFM3dRyY7Na900IttREK%2Fimg.png)

- HTTP 헤더에서 content-type, 길이, 캐시전략, 유효성 토큰 등 다양한 정보를 내려준다.

---

## Etags로 캐시 검증

- 서버는 Etag를 HTTP 헤더에 담아 유효 토큰으로 통신
- 유효 토큰은 효율적인 자원 업데이트 체크를 가능하게 한다. 리소스가 바뀌지 않는다면 아무 데이터도 전달되지 않는다.

- 위 그림의 경우에서, 120초가 지났다고 가정하고 같은 자원에 대해 새로운 요청을 보냈다고 가정하자.
  - 첫 째로, 브라우저는 로컬 캐시를 확인하고 이전의 응답을 찾아보지만, 기간이 만료 되어 존재하지 않는다.
  - 둘 째로, 브라우저는 새로운 요청을 보내고 응답을 기다리는데, 이는 비효율적이다. 이미 캐시에 있는 자원을 또 다운로드 하기 때문.

- Etags 헤더를 정의해 문제를 해결
  - 서버에서 중재 토큰을 생성해 파일에 대한 해시값 또는 파일 내용에 대한 지문(식별자)이다.
  - 이를 서버에 다음번 요청때 보내 식별자가 같다면, 리소스가 바뀌지 않았다는 것이므로 다운로드를 건너뛴다.

![Etags](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FblpjTi%2FbtqyAuBhGSw%2F9K0PBLaIbeDFd9NTVQ6oz0%2Fimg.png)

- 클라이언트는 Etag 토큰을 __Http request__ 헤더의 __If-None-Match__ 에 담아서 보낸다.
- 서버는 토큰을 현재 자원과 비교하여 토큰이 바뀌지 않았다면 __304 Not Modified__ 를 리턴한다.
- 클라이언트는 캐시에서 바뀐것이 없고 다시 120초의 시간을 재할당 하라고 알려준다.

- 효과적인 재검증을 어떻게 할 수 있을까?
  - 브라우저가 이런 일을 대신 해준다!
  - 자동으로 유효토큰이 이미 정의되었는지 검사해주고, 다시 보낼 요청에 붙여주고, 타임스탬프를 업데이트 한다.
  - 우리가 할 일은 서버에 필요한 __Etags__ 토큰만 내려주면 된다!

---

## Cache-Control

- 각 자원은 HTTP 헤더의 __Cache_control__ 을 통해서 정의된다.
- __Cache-control__ 속성은 누가 응답을 어떤 조건에서, 얼마나 캐시할 수 있는지 정의

- 가장 좋은 요청은 서버와 통신하지 않는 요청.
- 

![cache-control](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdF1e8x%2FbtqyAwsllf5%2F6SLeMGtISnCTd3TSh92lf0%2Fimg.png)

---

## no-cache, no-store

- __no-cache__ 는 반환된 응답이 자원이 바뀌었다면 서버의 첫 번째 응답체크를 하지 않고서야 이어지는 요청을 만족시킬 수 없다는 것?
  - 즉 __요청할 때마다 매번 Etag를 검사한다__ 는 것.
  - __max-age가 0__ 라는 의미와 같다.

  - 매번 Etag를 검사하기 때문에 자원이 바뀌었다면 바로 다운로드가 가능하고, 바뀌지 않았다면 다운로드를 피함

- 결과적으로 적절한 유효토큰(Etags)가 존재하면, no-cache는 캐시 응답을 체크하기 위해 왕복 통신이 발생
- 하지만 자원이 바뀌지 않았을 때 다운로드를 피할 수 있음.
  - 캐시가 있다면 요청 자체를 하지 않지만, no-cache로 한다면 매번 요청을 진행해 Etags를 검사하는것

---

## public vs private

- 응답이 __public__ 이라면 응답 코드가 정삭적으로 캐시할 수 있다는 코드가 아니더라도, HTTP 검증과 연관되어 있더라도 어찌됐건 캐시를 할 수 있다는 의미
  - 대부분 public은 필요가 없다. max-age 와 같은 설정을 통해 명시적으로 캐싱할 수 있었고 응답이 어쨌든 캐시될 수 있다는 의미이기 때문

- 브라우저는 __private__ 응답 캐시를 할 수는 있지만 응답은 전형적으로 __싱글 유저(=말단 유저)__ 를 타겟으로 하고 중간 매개체들은 캐시할 수 없다는 의미
  - 개인정보가 있는 HTML를 캐시할 수는 있지만 CDN은 페이지를 캐시할 수 없다는 의미

---

## 최적의 Cache-Control 전략 세우기

![캐시 계층구조](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fnd3WM%2FbtqyzcuzmUG%2FhjCOC25nWDkCN6SI4OEZp0%2Fimg.png)


- 재사용 가능한 응답인가?
  - (X) => no-store

- 매번 검증을 해야 하는가?
  - (O) => no-cache

- 중간 매개체가 캐시 해도 되는가?
  - (X) => private
  - (O) => public

- 캐시의 유효기간 설정 => max-age -> Etag 헤더 추가

### Chche-Control directives 예시

    max-age = 86400

- 응답을 하루(1 day)로 캐시하도록 설정

    private, max-age=600

- 응답을 클라이언트 브라우저(말단)에 10분으로 설정

    no-store

- 응답은 캐시를 허용하지 않으며 매 요청마다 갱신

- 아카이브에 따르면 통상 응답에 대한 다운로드들 중 절반을 캐시한다.

---

## 캐시 만료기간 전 자원이 업데이트 된다면?

- 캐시 응답을 업데이트 하거나 비활성화 하려면?

- 브라우저가 응답을 캐시한 이후 캐시기간이 만료하기 전까지 혹은 어떤 이유에서 캐시에서 삭제되거나 할 때까지 사용
- 결과적으로 다양한 사용자들은 완성단계에 이르기 전까지 다른 버전의 리소스들을 사용하게 됨
  - 어떤 사용자는 최신 버전의 자원을 사용, 어떤 사람들은 이전에 캐시된 상대적으로 오래된 자원을 사용

- 리소스의 URL을 바꾸고 컨텐츠가 바뀌었다면 새로 다운받게 강제할 수 있다.
  - 전형적으로, 파일의 식별자(또는 version number)를 파일 이름에 삽입하면 된다.

![리소스당 캐시 전략](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbJ0vp5%2FbtqyzwzBqeh%2F6ldCVkLGTwE0BIFV4ElHx1%2Fimg.png)

- 리소스 당 캐시 전략을 정의한다는 것은 __cache hierarchies__ 캐시 계층 구조를 가능하게 해줌
- 각각이 얼마동안 캐시가 되는지 설정할 수 있을 분만 아니라 방문자들로 하여금 새로운 버전을 얼마나 빠르게 볼 수 있는가?


- HTML은 __no-cache__ 로 정의를 한다.
  - 브라우저는 항상 재검증을 하게 하고 최신 버전의 자원을 각 요청마다 받아올 수 있게 한다.
  - 또한 HTML 코드 상에 CSS Javscript 자원에 대한 식별자를 임베드.
  - 서버 측에서 자원이 변경되면 HTML도 새로운 자원이 다운로드 되기 때문에 바뀜

- CSS는 브라우저들과 중간 캐시들 까지 1년동안 캐시할 수 있게 허용.
  - 1년간의 안전한 __far future expires__ 전략을 사용할 수 있다.
  - 파일의 식별자를 임베드 해놨기 때문
  - CSS가 업데이트 되면 URL도 바뀌게 된다.

- Javascript도 마찬가지로 1년으로 설정되었지만 __private__ 이다.
  - CDN에서 캐시 하면 안 될 개인정보가 포함되어 있을 것

- Image는 버전이나 고유한 식별자 없이 1년 캐시가 설정

- Etage, Cache-Control 그리고 고유한 URL들의 조합으로 long-lived 한 만료시간과 응답에 대한 캐시 컨트롤, 그리고 언제든 가능한 업데이트를 가능하게 한다.

---

## Caching 체크 리스트

- 캐시 전략에 베스트는 없다.


- 일정한 URL을 써라.
  - 같은 자원을 다른 URL로 서비스 하고 있다면 그 자원은 여러번 호출된다.

- 서버 측에서 유효토큰(Etags)를 제공하게 하라.
  - 유효 토큰은 서버에서 자원이 바뀌지 않을 때 같은 바이트들을 통신하는걸 막아준다.

- 어떤 자원들이 중간 저장을 가능하게 할지 결정해라
  - 모든 사용자들에게 동일한 자원은 CDN이나 다른 중간 매개체들에게도 캐시할 수 있게 하는 좋은 후보다.

- 각 자원의 최적의 캐시 기간을 정해라
  - 다양한 자원은 각기 다른 신선도를 요구, 각각의 max-age를 설정해라

- 당신의 사이트에 최적의 캐시 계층구조를 결정해라.
  - 리소스 URL과 자원의 식별자 HTML 문서에 대한 short나 no-cache 전략으로 업데이트에 빠르게 대응

- churn(휘젓기)? 를 최소화 해라
  - churn : 휘젓기 - 파일의 분류를 잘 해 놔라!
  - 어떤 리소스는 다른 것보다 자주 바뀜.
  - 만약 리소스의 특정 부분이 자주 업데이트 된다면 따로 파일을 분류해서 내려주는 것이 낫다.
  - 나머지 컨텐츠들에 대해서는 업데이트 될 때 캐시로부터 다운로드를 줄일 수있다.
  