---
layout: post
title: "2023/01/13 블로그 업데이트"
excerpt: "2023/01/13 블로그 업데이트 사항"
categories: ['Blog']
last_modified_at: 2023-01-13
published: True
---

## 블로그 이름 변경

_Big-Note -> De-Note_

원래는 블로그를 큰 메모장으로 사용하겠다는 의미로 블로그에 Big note란 이름을 붙였다. 그런데 뭔가 더 개발 블로그에 맞는 이름으로 바꾸고 싶어서 생각하다가 developer의 note란 의미로 De Note란 이름이 떠올랐다. 마침 영어단어 denote에 표시하다, 나타내다라는 의미도 있어서 나 자신을 나타낸다는 의미로도 쓸 수 있을 것 같아서 괜찮은 이름인 것 같다.

## Baseurl 추가

_https://sparkafka.github.io/ -> https://sparkafka.github.io/de-note/_

나중에 깃헙에서 개발 블로그 말고 다른 사이트를 배포할 수 있다고 생각해서 바꾸게 되었다. 이질적인 리포지토리 이름(sparkafka.github.io)도 한 몫 했다.

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
