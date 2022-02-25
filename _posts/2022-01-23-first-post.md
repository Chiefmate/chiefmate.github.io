---
layout: post
title: "markdown 이용하기"
tags: blog github_pages
---

# jekyll에서 markdown을 이용해보자!

## 출처

[Github Pages Docs](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
에서 가져왔다.

[Jekyll Markdown Reference](https://www.markdownguide.org/tools/jekyll/)도 참고했다.

## 왜 쓰는걸까?

마크다운은 html보다 훨씬 쉽고 간단한 마크업 언어다.
한글, MS Word 등으로 작성하여 html 문서로 바꾸는 것보다 훨씬 간결하게 html 문서를 작성할 수 있다.
구글 등 검색엔진 최적화에도 유리하다.

단점도 있다. 표준이 없다는 것.. VScode에서 작성하며 Preview 확장으로 확인한 내용이 jekyll에서는 원하던 모양이 아닐 때도 있었다.

공부 목적으로 많이 사용하는 기능들을 모아 정리했다.
아래의 내용은 jekyll의 기본설정을 기준으로 작성되었음을 밝힌다.

## 헤더 (Header)

글의 내용 앞에 `#`와 띄어쓰기를 하면 헤더를 쓸 수 있다.
`#`의 개수에 따라 제목 수준을 다르게 할 수 있다.

~~~
# 제목 수준 1
## 제목 수준 2
### 제목 수준 3
#### 제목 수준 4
##### 제목 수준 5
###### 제목 수준 6
~~~
실행 결과
# 제목 수준 1
## 제목 수준 2
### 제목 수준 3
#### 제목 수준 4
##### 제목 수준 5
###### 제목 수준 6


---

## 코드블럭 이용하기

<code>```</code>를 이용하여 앞 뒤 한 줄씩 붙여준다.

~~~
```
int	main(void)
{
	return (0);
}
``` 
~~~
**실행 결과**
```
int	main(void)
{
	return (0);
}
``` 

### 언어 별 문법 하이라이트
<code>``` c</code>와 같이 <code>```</code>뒤에 언어를 붙여준다. jekyll이 `redcarpet`을 지원하면서 가능해졌다고 한다.

**Ruby 코드**
~~~
``` ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!");
puts markdown.to_html
```
~~~
**출력 결과**
``` ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!");
puts markdown.to_html
```

**C 코드**
```
	``` c
	int	main(void)
	{
		return (0);
	}
	```
```
**출력 결과**
```c
	int	main(void)
	{
		return (0);
	}
```

---

## 인라인 코드블럭 (Quoting code)

\`를 앞 뒤에 붙여 문장 안에서 코드블럭을 씌울 수 있다.

**코드**
~~~
이 포스트는 `VScode`로 작성했지만, 편리한 마크다운 에디터가 많다고 한다. `Prose.io`, `StackEdit` 등이 있다고 한다. `VScode`에서는 `Preview`확장으로 글을 쓰며 html 문서를 바로 확인할 수도 있다.
~~~
**실행 결과**
이 포스트는 `VScode`로 작성했지만, 편리한 마크다운 에디터가 많다고 한다. `Prose.io`, `StackEdit` 등이 있다고 한다. `VScode`에서는 `Preview`확장으로 글을 쓰며 html 문서를 바로 확인할 수도 있다.

---

## 볼드체, 이탤릭체, 취소선 등 Styling text

볼드체는 `**` 또는 `__`, 이탤릭체는 `*` 또는 `_`, 취소선은 `~~`으로 감싸서 활용할 수 있다.
볼드체와 이탤릭체를 혼용할 때는 `*`와 `_`를 같이 사용하면 된다.
모두 볼드체와 이탤릭체를 적용할 때는 `***`로 감싼다.

**코드**
~~~
**볼드체**와 *이탤릭체*, ~~취소선~~입니다.
**볼드체와 _이탤릭체_를**
__*혼용*을 할 수 있습니다.__
***모두 볼드체와 이탤릭체를 적용했습니다.***
~~~

**실행 결과**
**볼드체**와 *이탤릭체*, ~~취소선~~입니다.
**볼드체와 _이탤릭체_를**
__*혼용*을 할 수 있습니다.__
***모두 볼드체와 이탤릭체를 적용했습니다.***

---

## 링크

`[표시될 내용](URL)`의 형식으로 인라인 링크를 쓸 수 있다.

**코드**
~~~
[여기](https://chiefmate.github.io)를 눌러 링크로 이동하세요.
~~~
**실행 결과**
[여기](https://chiefmate.github.io)를 눌러 링크로 이동하세요.

---

## 이스케이프 문자

`\`를 아래의 문자 앞에 붙여서 markdown 문자들을 이스케이프 할 수 있다.
~~~
\   backslash
`   backtick
*   asterisk
_   underscore
{}  curly braces
[]  square brackets
()  parentheses
#   hash mark
+   plus sign
-   minus sign (hyphen)
.   dot
!   exclamation mark
~~~
출처는 [여기](https://daringfireball.net/projects/markdown/syntax#backslash).
예를 들어 \#를 쓰고 싶다면 `\#`로 입력하는 식이다.

한편 이 글을 작성하면서 <code>```</code>를 사용하는 코드블럭 안에 이스케이프해야하는 문자들을 집어넣느라 많은 삽질을 했다.
배운 것들을 아래에 정리해보았다.

backtick (`)를 이스케이프해서 코드블럭안에 넣으려면...
1. 인라인 코드블럭
`<code> </code>`를 사용한다.
2. 통 코드블럭
`~~~`를 <code>```</code>대신 사용한다.

## 추가할 내용

리스트, 표, 이미지 등 중요한 기능들이 많이 남아있다. 추가로 정리해볼 예정이다.