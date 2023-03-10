---
layout: post
title: "2023/03/11 블로그 업데이트 - code block 스크롤 추가"
excerpt: "2023/03/11 블로그 업데이트 사항"
categories: ['Blog']
last_modified_at: 2023-03-11
published: True
---

## Code Block 스크롤 추가

![scroll](/de-note/assets/images/22nd/scroll.png)

코드가 코드 블럭 크기를 넘어가면 줄바꿈이 되어서 보기가 불편했는데, 코드가 코드 블럭 크기를 넘어가면 자동으로 스크롤을 생성하도록 변경하였다. `custom.css`에 `.highlight pre` 태그를 변경해주었다. lanyon 테마에서는 `.highlight pre` 태그를 변경해주어야 한다.

```css
/* custom.css */

.highlight pre {
  word-wrap: normal;
  overflow-x: auto;
  white-space: pre;
}
```