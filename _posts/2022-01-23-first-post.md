---
layout: post
title: "markdown 이용하기"
tags: blog github_pages
---

# 첫 글

## 출처
[Github Pages Docs](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
에서 가져왔다.

---

## 이스케이프 문자



## 코드블럭 이용하기

`\`\`\``를 이용하여 앞 뒤 한 줄씩 붙여준다.

```
int	main(void)
{
	return (0);
}
```


### 언어 별 문법 하이라이트
"``` c"와 같이 뒤에 언어를 붙여준다. jekyll이 `redcarpet`을 지원하면서 가능해졌다고 한다.
``` ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!");
puts markdown.to_html
```

```c
int	main(void)
{
	return (0);
}
```

## 2단계제목

**이건** 첫 글 입니다. 
문단 구분
