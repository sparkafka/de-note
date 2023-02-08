---
layout: post
title: "[Python] Datapane으로 작성한 Report의 pdf 변환"
excerpt: Datapane으로 작성한 Report를 pdf로 변환해보자.
categories: ['Python']
last_modified_at: 2023-01-18
published: True
---

## 들어가며

[지난 포스트](https://sparkafka.github.io/de-note/7-python-datapane/)에서 Python Datapane 라이브러리로 HTML 형식의 Report를 작성해 보았다. 이제 작성한 Report HTML 파일을 pdf로 바꾸기 위해 기존에 존재하던 HTML to pdf 라이브러리를 사용했는데, 변환이 안되는 것이었다! 그래서 나는 다른 방법을 찾아야 했다...

<br/>

## 1. 기존의 HTML to pdf 라이브러리

Python에서 사용할 수 있는 HTML to pdf 라이브러리를 대표적으로 wkhtmltopdf를 이용하는 라이브러리은 pdfkit과 weasyprint 라이브러리가 있다. 나는 먼저 pdfkit을 가지고 파일 변환을 해보았다. pdfkit을 사용하려면 먼저 wkhtmltopdf를 설치해야 한다.

![기존 html 파일](/de-note/assets/images/8th/table-test.jpeg)


```python
import pdfkit
pdfkit.from_file('table-test.html', 'table-test.pdf')
```
![pdfkit 결과](/de-note/assets/images/8th/table-test-pdf.jpg)

그런데 위와 같이 백지로만 나오고 변환이 안 되는 것이었다! weasyprint를 사용해봤지만 결과는 그대로였다.

<br/>

- 원인?

![report 코드 일부](/de-note/assets/images/8th/report-code.png)

위 캡쳐본은 table-test.html 코드의 일부분이다. 간단한 report임에도 불구하고 ```<script>``` 태그가 상당히 길고 복잡한 것을 알 수 있다. 나도 잘 모르지만, 이런 복잡한 script 때문에 변환이 안 되는 것 아닐까라는 생각을 하였다.   

그래서 나는 다른 방법을 찾아야 했다. 바로 변환이 안 된다면, html을 이미지로 캡쳐하고 그걸 pdf로 바꾸면 되지 않을까? 라는 생각을 하였다.

<br/>

## 2. HTML to image

Html 파일을 이미지로 캡처를 하기 위해 Python Html2image 라이브러리를 설치하고 테스트해보았다. Html2image 라이브러리는 html 파일이나 웹페이지를 캡처형식으로 이미지로 변환시켜주는 Python 라이브러리이다. Html2image 라이브러리는 크롬에서 스크린샷을 찍는 형식으로 사용되기 때문에 라이브러리를 사용하려면 먼저 크롬을 설치해야 한다.

```python
from html2image import Html2Image

hti = Html2Image()
source_html = open('table-test.html', 'rt', encoding='utf-8').read()
hti.screenshot(html_str=source_html, save_as='table-html2image.png',size=(1200,1500))
```

Html2image에서는 사이즈를 정하면 그 사이즈대로 html 파일이나 웹페이지를 캡처하여 이미지로 저장한다.

![Html2image 결과](/de-note/assets/images/8th/table-html2image.png)

실행한 결과, 위와 같이 이미지 파일로 변환이 되었다! 이제 이 이미지 파일을 pdf로 변환시키자.

<br/>

## 3. Image to pdf

이미지를 pdf 파일로 바꾸기 위해 Python img2pdf 라이브러리를 이용하였다.

```python
import img2pdf
with open('table-img2pdf.pdf', 'wb') as file:
    file.write(img2pdf.convert('table-html2image.png'))
```

![Img2pdf 결과](/de-note/assets/images/8th/html2pdf.jpg)

위와 같이 간편하게 이미지 파일이 pdf로 변환이 되는 것을 확인할 수 있다. 이렇게 Datapane Report Html 파일을 pdf로 변환하는 작업이 완료되었다.

<br/>

## 마치며

처음에 Report html 파일이 pdf 변환 라이브러리로 변환이 안 될때 많이 당황했는데, 이렇게 해결할 방법을 찾은게 천만다행이라고 생각했다. 그 덕분에 나름 많은 것을 알 수 있어서 나쁘지만은 않은 경험이었지만, Datapane 자체적으로 파일 변환 기능을 넣어줬으면 한다. 이 방법도 결국에는 꼼수에 가까운 방법이니... 빠른 기능 추가를 바란다.

## Reference

- [https://datapane.com/](https://datapane.com/)   
- [https://pypi.org/project/html2image/](https://pypi.org/project/html2image/)   
- [https://pypi.org/project/img2pdf/](https://pypi.org/project/img2pdf/)
