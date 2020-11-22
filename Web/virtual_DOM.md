# Virtual DOM

- vue 기준으로 설명한다.

- 가상 DOM을 지원해 아주 빠른 UI 렌더링 속도를 보여준다.

- HTML 문서가 있을 때 이를 DOM Tree로 표현할 수 있다.

- 여기서 새로운 Element를 추가하거나 변경하려면, 해당 객체를 찾고, 새로운 element를 생성하고 attribute나 element를 업데이트 하는 DOM API를 사용해야 한다.

```js
const listItemOne = document.getElementByClassName("list_item")[0];
listItemOne.textContent = "List item one";

const list = document.getElementByClassName("list")[0];
const listItemTwo = document.createElement("li");
listItemTwo.classList.add("list_item");
listItemTwo.textContent = "List item two";
list.appendChild(listItemTwo);
```

- `document.getElementByClassName()`의 경우 DOM의 크기가 작을 때는 상관 없지만, 몇 초 간격으로 다양한 element를 변경하기 시작 할 경우 DOM에 update하는 쿼리 비용이 비싸지게 된다.
- 일반적으로 HTML 문서에서 특정 elements들을 찾아 업데이트 하는 비용보다는 문서의 큰 부분을 업데이트 하는게 더 간단

```js
const list = document.getElementByClassName("list")[0];
list.innerHTML = `
<li class="list_item">List item one</li>
<li class="list_item">List item two</li>
```

- 웹 페이지가 거대해질 수록 원하는 부분만 찾아서 업데이트 하는 것이 성능상에 매우 중요한 요소가 된다.

---

## _Virtual DOM_

- DOM을 빈번히 업데이트 하는 것을 좀 더 효율적인 방법으로 업데이트 하기 위해서 고안
- React나 Vue에서 채택 해 유명해짐
- Virtual DOM은 실제 DOM의 복사본이라고 생각
- 실제 DOM API 호출 없이 빈번하게 조작 및 변경이 가능
- 모든 변경이 Virtual DOM에 반여이 된 후, 실제 DOM을 효율적으로 변경

<p align="center">
    <img src="https://miro.medium.com/max/283/1*YGDRSlZoTAq0bC0qvHE4aw.png">
</p>

- 위와같은 구조를 DOM Tree를 Javascript 객체로 표현 할 수 있다.

<p align="center">
    <img src="https://miro.medium.com/max/619/1*Ck7mYj3asdlAgFGP1B_UmA.png">
</p>

- 위 Javascript 객체를 Virtual DOM이라고 생각하면 된다.
- Javascript 객체이기 때문에 실제 DOm에 대한 조작 없이 자유롭고 빈번하게 조작할 수 있다.
- 실제로 Virtual DOM을 조작할 때는 전체가 아닌 작은 영역으로 쪼개어 작업하는게 일반적
- 위에서 list의 업데이트와 추가를 했기 때무에 아래와 같이 쪼갤 수 있다.

<p align="center">
    <img src="https://miro.medium.com/max/621/1*cBE2bTG3CeFYIQMz_c5B7g.png">
</p>

- 실제 DOM을 조작하는게 아니라 Virtual DOm을 사용했을 때, 어떻게 성능에 도움이 되는지 확인해보자.
- 우선 List item -> List item one으로 변경하고 두번째 List를 List item two라는 내용이 반영된 Virtual DOM의 복사본을 아래와 같이 생성한다.

<p align="center">
    <img src="https://miro.medium.com/max/621/1*5C7uC5ihVF7BIqlFaNBJBg.png">
</p>

- List item one 으로 변경이 되었고 List item two 가 추가 되어 있는 Virtual DOM의 복사본입니다.
- 이 복사본과 Virtual DOM을 비교해서 실제로 어디가 달라졌는지를 알 수 있는 diff를 만들 수 있습니다.

<p align="center">
    <img src="https://miro.medium.com/max/627/1*Re4fNPRpz35djeudPzj7Pw.png">
</p>

- 이 diff 는실제 DOM을 어떻게 변경해야 할지 알려줍니다.
- 모든 diff가 만들어진 후에 필요한 update를 실제 DOM에 반영합니다.
- 아래와 같이 loop를 돌며 추가된 자식들은 추가하고, 변경된 자식들은 update 하게 됩니다.

<p align="center">
    <img src="https://miro.medium.com/max/622/1*d-eQz9z-a-KvOmYyn35Glg.png">
</p>