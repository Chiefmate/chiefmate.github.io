---
layout: post
title: "markdown 이용하기"
tags: blog github_pages
---

# 첫글

## 코드블럭 이용하기

```
\`\`\`
```를 이용하면 된다.

```c
int	main(void)
{
	return (0);
}
```


## 언어 별 문법 하이라이트
``` ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!");
puts markdown.to_html
```

```c
int	main(void)
{
	return (0)l
}
```

## 2단계제목

**이건** 첫 글 입니다. 
문단 구분
