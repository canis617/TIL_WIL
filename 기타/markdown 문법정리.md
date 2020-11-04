# __MarkDown__

## __장점__

- 문법이 쉽다
- 관리가 쉽다
- 지원 가능한 플랫폼, 프로그램이 다양하다.

## __단점__

- 표준이 없어 사용자마다 문법이 상이
- HTML 마크업 대체 x

# __문법 (Syntax)__

</br>
## __제목(Header)__

```markdown
#       제목 1 <h1>

제목 1 
======

##      제목 2 <h2>

제목 2
------

###     제목 3 <h3>
####    제목 4 <h4>
#####   제목 5 <h5>
######  제목 6 <h6>
```

#       제목 1

제목 1 
======

##      제목 2

제목 2
------

###     제목 3
####    제목 4
#####   제목 5
######  제목 6

</br></br>
## __강조(Emphasis)__

``` markdown
기울이기는 *별표* 또는 _언더바_ 를 사용
두껍게는 **별표두개** 또는 __언더바두개__ 를 사용
취소선은 ~~물결표시~~ 를 사용
<u>밑줄</u>은 '<u></u>'를 사용
```
기울이기는 *별표* 또는 _언더바_ 를 사용  
두껍게는 **별표두개** 또는 __언더바두개__ 를 사용  
취소선은 ~~물결표시~~ 를 사용  
<u>밑줄</u>은 ```'<u></u>'```를 사용  

</br></br>
## __목록(List)__

```markdown
1. 순서가 필요한 목록
1. 순서가 필요한 목록
  - 순서가 필요x
  - 순서가 필요x
1. 순서가 필요한 목록
  1. 순서가 필요한 목록(서브)
  1. 순서가 필요한 목록(서브)
1. 순서가 필요한 목록
  * 순서가 필요하지 않은 목록
  - 순서가 필요하지 않은 목록
  + 순서가 필요하지 않은 목록
```
1. 순서가 필요한 목록
1. 순서가 필요한 목록
    - 순서가 필요x
    - 순서가 필요x
1. 순서가 필요한 목록
   1. 순서가 필요한 목록(서브)
   1. 순서가 필요한 목록(서브)
- 순서가 필요하지 않은 목록에 사용 가능한 기호
  * 순서가 필요하지 않은 목록
  - 순서가 필요하지 않은 목록
  + 순서가 필요하지 않은 목록

</br></br>
## __링크(Links)__
```markdown

[GOOGLE](https://google.com)

[NAVER](https://www.naver.com "부가설명")

[상대적 참조](../)

[Dribbble][Dribbble link]

[Github][1]

이 외에도 URL에 꺽쇠 <>를 사용함으로서 자동인식

구글 홈페이지 : https://google.com
네이버 홈페이지 : <https://www.naver.com>

[Dribbble link] : https://dribbble.com
[1] : https://canis617.github.com
[참조 링크] : https://www.naver.com "부가설명"

```

[GOOGLE](https://google.com)  

[NAVER](https://www.naver.com "부가설명")  

[상대적 참조](../)  

[Dribbble][Dribbble link]  

[Github][1]  

이 외에도 URL에 꺽쇠 <>를 사용함으로서 자동인식  

구글 홈페이지 : https://google.com  
네이버 홈페이지 : <https://www.naver.com>  

[Dribbble link]: https://dribbble.com  
[1]: https://canis617.github.com  
[참조 링크]: https://www.naver.com "부가설명"  

## __이미지 (Images)__

```markdown
<img> 사용가능

![대체 텍스트 입력](http://www.gstatic.com/webp/gallery/5.jpg "부가설명")

![Kayak][logo]

[logo]: http://www.gstatic.com/webp/gallery/2.jpg "To go kayaking."

```
![대체 텍스트 입력](http://www.gstatic.com/webp/gallery/5.jpg "부가설명")

![Kayak][logo]

[logo]: http://www.gstatic.com/webp/gallery/2.jpg "To go kayaking."

### 이미지에 링크

```markdown

[![Kayak](http://www.gstatic.com/webp/gallery/2.jpg)](https://www.naver.com "부가설명")

```

[![Kayak](http://www.gstatic.com/webp/gallery/2.jpg)](https://www.naver.com "부가설명")

</br></br>

## __코드강조__

`<pre>, <code>`로 변환 가능  
1 왼쪽의 ` 사용

### __인라인 코드 강조__
```markdown

`background` 혹은 `background-image` 속성으로 배경이미지 삽입가능

```

`background` 혹은 `background-image` 속성으로 배경이미지 삽입가능

### __블록 코드 강조__
` 를 3개이상 사용, 코드 종류 작성

```markdown

    ```html
    <a href="https://www.google.co.kr">GOOGLE</a>
    ```

    ```css
    .list > li {
        position: absolute;
        top: 40px;
    }
    ```

    ```javascript
    function func(a, b) {
        return a+b;
    }
    ```

    탭 으로도 코드 종류 상관없는 블록 코드 강조 가능

```

```html
<a href="https://www.google.co.kr">GOOGLE</a>
```

```css
.list > li {
    position: absolute;
    top: 40px;
}
```

```javascript
function func(a, b) {
    return a+b;
}
```

## __표, 테이블 (Table)__

`<table>` 태그 변환

헤더 /셀을 구분 할 때 3개 이상의 - (hyphen/dash) 기호가 필요하다.  
헤더 셀을 구분하면서 `:` (Colons) 기호로 셀(열/칸) 안에 내용을 정렬 할 수 있다.  
가장 좌측과 가장 가장 우측에 있는 | (Vertical bar)는 생략 가능  

```markdown

| 값 | 의미 | 기본값 |
|---|:---:|---:|
| `static` | 유형(기준) 없음 / 배치 불가능 | `static` |
| `relative` | 요소 자신을 기준으로 배치 |  |
| `absolute` | 위치 상 부모(조상)요소를 기준으로 배치 |  |
| `fixed` | 브라우저 창을 기준으로 배치 |  |

값 | 의미 | 기본값
---|:---:|---:
`static` | 유형(기준) 없음 / 배치 불가능 | `static`
`relative` | 요소 **자신**을 기준으로 배치 |
`absolute` | 위치 상 **_부모_(조상)요소**를 기준으로 배치 |
`fixed` | **브라우저 창**을 기준으로 배치 |

```

| 값 | 의미 | 기본값 |
|---|:---:|---:|
| `static` | 유형(기준) 없음 / 배치 불가능 | `static` |
| `relative` | 요소 자신을 기준으로 배치 |  |
| `absolute` | 위치 상 부모(조상)요소를 기준으로 배치 |  |
| `fixed` | 브라우저 창을 기준으로 배치 |  |

값 | 의미 | 기본값
---|:---:|---:
`static` | 유형(기준) 없음 / 배치 불가능 | `static`
`relative` | 요소 **자신**을 기준으로 배치 |
`absolute` | 위치 상 **_부모_(조상)요소**를 기준으로 배치 |
`fixed` | **브라우저 창**을 기준으로 배치 |

</br></br>
## __인용문(BlockQuote)__

```markdown

> 남의 말을 인용
> _~~_

BREAK!

> 인용문을 작성
>> 중첩된 인용문 1
>>> 중첩2된 인용문 1
>>> 중첩2된 인용문 2
>>> 중첩2된 인용문 3

```

> 남의 말을 인용
> _~~_

BREAK!

> 인용문을 작성
>> 중첩된 인용문 1
>>> 중첩2된 인용문 1  
>>> 중첩2된 인용문 2  
>>> 중첩2된 인용문 3  

## __원시 HTML(Raw HTML)__

원시 HTML문법은 Markdown에서 작용

```markdown

<u> 마크다운에서 지원하지 않는 기능</u>을 사용

<img width="150" src="http://www.gstatic.com/webp/gallery/4.jpg" alt="Prunus" title="A Wild Cherry (Prunus avium) in flower">

![Prunus](http://www.gstatic.com/webp/gallery/4.jpg)

```
<u> 마크다운에서 지원하지 않는 기능</u>을 사용

<img width="150" src="http://www.gstatic.com/webp/gallery/4.jpg" alt="Prunus" title="A Wild Cherry (Prunus avium) in flower">

![Prunus](http://www.gstatic.com/webp/gallery/4.jpg)  


## __수평선(Horizontal Rule)__

기호를 3개이상 사용

```markdown

기호 : - * _

---
(Hyphens)

***
(별표)

___
언더바
```
---
(Hyphens)

***
(별표)

___
언더바

## __줄 바꿈(Line Breaks)__

```markdown
동해물과 백두산이  <!-- (띄워쓰기 두번) -->
마르고 닳도록
하느님이 보우하사 <!-- 안하면 붙어서 나옴 -->  
대한사람 대한으로 길이 보전하세

또는 <br>사용
```

동해물과 백두산이  <!-- (띄워쓰기 두번) -->  
마르고 닳도록
하느님이 보우하사 <!-- 안하면 붙어서 나옴 -->  
대한사람 대한으로 길이 보전하세

