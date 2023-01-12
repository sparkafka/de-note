---
layout: post
title: "[Python] Altair에서 tick label에 단위를 추가하자"
excerpt: "Altair chart에서 tick에 단위 추가하기"
categories: ['Python']
last_modified_at: 2023-01-13
published: True
---

## 들어가며

Python에서 차트를 그리기 위해 [Altair 라이브러리](https://altair-viz.github.io/)를 사용하는데, 차트의 tick에 단위를 추가할 일이 생겼다. 그래서 이곳 저곳 찾아보았다.

<br/>

## label에 단위 추가하기

단위를 추가하는 법을 알아보기 위해 먼저 Altair 공식 페이지의 다음 [코드](https://altair-viz.github.io/gallery/simple_bar_chart.html)를 사용하였다.

```python
import altair as alt
import pandas as pd

source = pd.DataFrame({
    'a': ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I'],
    'b': [28, 55, 43, 91, 81, 53, 19, 87, 52]
})

alt.Chart(source).mark_bar().encode(
    x='a',
    y='b'
)
```

![Altair default](/images/12th/alt_default.png)


y label에 단위를 추가하려면 다음과 같이 ```altair.Y```의 ```labelExpr``` 파라미터에 ```"datum.value + '단위'"``` 식으로 넣으면 된다고 한다. 단위로 '개'를 추가한 모습이다. 

```python
...
    y=alt.Y("b", axis=alt.Axis(labelExpr="datum.value + '개'"))
...
```

![Altair add unit](/images/12th/alt_gae.png)

이렇게 직접적으로 문자열을 파라미터로 넣어주어 단위를 추가할 수 있었다. 그런데 나는 변수를 이용하여 단위를 추가하고 싶어서 다음과 같은 코드를 사용하였다.

```python
...
unit = "개"
alt.Chart(source).mark_bar().encode(
    x='a',
    y=alt.Y("b", axis=alt.Axis(labelExpr="datum.value"+unit)
)
...
```

![Altair error1](/images/12th/alt_error1.png)

그런데 이게 웬걸? 갑자기 label이 없어졌다. 그래서 다르게 해봤다.

```python
...
unit = "개"
alt.Chart(source).mark_bar().encode(
    x='a',
    y=alt.Y("b", axis=alt.Axis(labelExpr="datum.value + unit"))
)
...
```

![Altair error2](/images/12th/alt_error2.png)

그랬더니 이제는 아얘 실행조차 되지 않았다. 그래서 나는 다음과 같이 성공했을 때 문자열을 따옴표까지 똑같이 해서 넣어야 겠다고 생각했다.

```python
...
unit = "m/s"
unitLabel = "datum.value + '" + unit + "'"
alt.Chart(source).mark_bar().encode(
    x='a',
    y=alt.Y("b", axis=alt.Axis(labelExpr=unitLabel))
)
...
```

![Altair success](/images/12th/alt_success.png)

그랬더니 추가한 단위가 잘 출력되는 것을 확인하였다. 변수로 단위를 넣고 싶을 때에는 위와 같은 형식으로 넣으면 될 것이다.

<br/>

## 마치며

왜 저렇게 들어가야 되는지는 잘 모르겠다. 자바스크립트 에러가 나오는 것으로 보아 자바스크립트와 관련된 것 같은데... 아무튼 성공해서 다행이다.   

## Reference

[https://altair-viz.github.io/gallery/simple_bar_chart.html](https://altair-viz.github.io/gallery/simple_bar_chart.html)   
[https://altair-viz.github.io/user_guide/generated/core/altair.Axis.html](https://altair-viz.github.io/user_guide/generated/core/altair.Axis.html)   
[https://stackoverflow.com/questions/69796996/to-show-bbillion-instead-of-ggiga-in-altair-charts-python](https://stackoverflow.com/questions/69796996/to-show-bbillion-instead-of-ggiga-in-altair-charts-python)

