---
layout: post
title: "2023/01/17 블로그 업데이트"
excerpt: "2023/01/17 블로그 업데이트 사항"
categories: ['Blog']
last_modified_at: 2023-01-18
published: True
---

## Permalink 수정

_pretty -> /:title/_

url 볼수록 길고 깔끔하지 않은 것 같아서 제목만 보이게 바꿨다. 페이지 색인 생성은 시간이 더 걸릴 것 같다... 

## 스타일 변경

_들여쓰기 추가_

들여쓰기는 진작 넣고 싶었는데, 프론트 관련 개발 지식이 없어서 ```%nbsp;``` 를 이용하여 빈칸을 강제로 넣고 있었다. 그런데 이번에 custom css 파일을 작성하고 적용하는 법을 알아내어서 들여쓰기를 추가하였다.    

현재 [lanyon](https://github.com/poole/lanyon) 테마를 사용중인데, lanyon 테마에는 ```poole.css```, ```syntax.css```, ```lanyon.css``` 파일들이 기본적으로 있는데 나는 이 파일들을 보존하기 위해 따로 커스텀 css 파일을 만들고(```custom.css```), 적용하였다. css 파일을 적용하기 위해서는 ```head.html``` 파일을 변경해주어야 한다.

```html
<!-- head.html -->

<link rel="stylesheet" href="{{ '/public/css/poole.css' | absolute_url }}">
<link rel="stylesheet" href="{{ '/public/css/syntax.css' | absolute_url }}">
<link rel="stylesheet" href="{{ '/public/css/lanyon.css' | absolute_url }}">
<link rel="stylesheet" href="{{ '/public/css/custom.css' | absolute_url }}">
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=PT+Serif:400,400italic,700%7CPT+Sans:400">
```

위에서 아래 순서대로 덮어쓰기가 된다고 한다. 그러니까 위 처럼 작성을 하면 같은 내용이 있으면 ```custom.css``` 파일의 우선순위가 더 높다. 위와 같이 수정을 하고 들여쓰기를 넣기 위해 ```custom.css``` 파일을 작성하였다.

```css
/* custom.css */

p {
  text-indent: 0.5em;
}
```

![indent](/de-note/assets/images/15th/indent.png)

들여쓰기 적용이 잘 된 모습이다.   

들여쓰기 적용 후, ```%nbsp;```를 이용하여 강제로 들여쓰기를 넣은 파일들을 수정하였다.   

## 메인 페이지 및 포스트 페이지 디자인 변경

_category 추가_

![main page](/de-note/assets/images/15th/main_page.png)

![post](/de-note/assets/images/15th/post.png)

위와 같이 메인 페이지와 포스트 페이지의 제목 위에 카테고리를 추가하였다. ```index.html``` 파일과 ```post.html``` 파일을 변경하였다.

```html
<!-- index.html -->

<span class="post-category">{{ post.categories }}</span>
    <h1 class="post-title">
        <a href="{{ post.url | absolute_url }}">
        {{ post.title }}
        </a>
    </h1>
    ...
```
```html
<!-- post.html -->

<span class="post-category">{{ page.categories }}</span>
<h1 class="post-title">{{ page.title }}</h1>
<span class="post-date">{{ page.date | date_to_string }}</span>
{{ content }}
...
```

제목 위에 카테고리를 넣었다. 카테고리 스타일도 역시 ```custom.css``` 파일에 추가하여 변경하였다. ```post.html``` 파일에서는 page를 써야 한다는 걸 몰라서 약간 당황했다.
