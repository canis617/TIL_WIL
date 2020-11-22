# Webhook과 polling

![webhook, pollin](https://miro.medium.com/max/700/1*WStVIykpS0sKLD2IFOxJUA.png)

- polling이란 우리의 앱에서 엔드포인트에 이벤트가 발생했는지 주기적으로 요청을 보내는 방식
- webhook은 엔드포인트에서 발생한 이벤트가 우리의 앱에 수신되는 형태

## webhook

- 이벤트에 대한 이벤트 핸들러로 생각하면 편하다.
- 이벤트 핸들러 안에서 다른 시스템으로 어떤 이벤트가 발생했는지에 대한 정보를 JSON이나 POST요청을 보내는 것

## webhook endpoint

- webhook endpoint는 그 발생한 이벤트가 어디로 전달되어야 하는가에 대한 것이다.
- 이벤트의 목적지로 보면 편하다.

![slack api webhooks](https://miro.medium.com/max/700/1*9jdCsMvsvXAJ3dpeMa-Z0Q.png)

- 슬랙 api는 incoming webhooks라는게 있다.
- 외부 시스템에서 슬랙으로 post message를 쏴주기 위한 간단한 방법
- 외부 시스템에서 어떤 이벤트가 발생해 웹훅이 실행되면 그 웹훅 내부에서 슬랙의 웹훅 엔드포인트로 post 요청을 쏘라는 말.
- 그러면 슬랙 채널에서 특정 메세지를 입력 할 수 있다.