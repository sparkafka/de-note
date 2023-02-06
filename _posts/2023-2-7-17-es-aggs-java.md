---
layout: post
title: "Elasticsearch Java High Level Client를 이용한 Aggregation"
excerpt: "Java High Level Client를 통해 Java로 Elasticsearch Aggregation을 수행해보자."
categories: ['Elasticsearch']
last_modified_at: 2023-02-05
published: False
---

## 들어가며

회사에서 Elasticsearch를 본격적으로 도입하면서 수집되는 데이터들에 대해 aggregation 작업을 수행해야 할 필요성이 생겼다. 그래서 공식 문서 등을 통해 rest api로 수행을 하는 법을 공부했다. 그런데 이것을 Java High Level Client로 수행을 하는 법을 정리한 곳을 찾기가 쉽지 않아 정리를 하면 좋겠다고 생각했다.

