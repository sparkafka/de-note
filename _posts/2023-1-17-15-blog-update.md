---
layout: post
title: "2023/01/17 블로그 업데이트"
excerpt: "2023/01/17 블로그 업데이트 사항"
categories: ['Blog']
last_modified_at: 2023-01-18
published: False
---

## Permalink 수정

_pretty -> /:title/_

url 볼수록 길고 깔끔하지 않은 것 같아서 제목만 보이게 바꿨다.

## 스타일 변경

_들여쓰기 추가_

들여쓰기는 진작 넣고 싶었는데, 프론트 관련 개발 지식이 없어서

## sitemap 변경 

### 1

// 이전

\{\{ site.url \}\}\{\{ post.url \}\}
    
// 이후

\{\{ site.url \}\}\{\{ site.baseurl \}\}\{\{ post.url \}\}

baseurl을 추가하고 사이트맵을 확인해보니 자동 생성되는 주소에 baseurl이 추가가 되지 않는다는 것을 확인하였다. 그래서 sitemap.xml 파일에서 주소를 담당하는 부분을 위와 같이 변경하였다.

### 2

_post.lastmod -> post.last_modified_at_

사이트맵에서 마지막 수정 날짜가 갱신이 안 되었는데, 변수명 설정이 달라서 그런 것이란 걸 확인했다. 현재 post front matter에 맞게 변경하였다.


