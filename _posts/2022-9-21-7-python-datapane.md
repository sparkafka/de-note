---
layout: post
title: "[Python]Datapane을 이용한 보고서 작성"
excerpt: Python Datapane 라이브러리를 사용해 HTML 보고서를 작성하자
categories:
    - Python
last_modified_at: 2022-09-22
published: false
---

## 들어가며

&nbsp; 회사에서 Python을 활용한 보고서 작성 프로그램을 만들어야 했다. 파이썬의 여러가지 라이브러리를 비교한 결과, Datapane이라는 라이브러리를 쓰기로 결정하였다. 이런 작업을 하는 것을 처음이라 여기저기 알아봤는데, Datapane에 대해 소개해주는 곳이 많이 없었다. 그래서 Datapane을 소개하기 위해 글을 작성하였다.
&nbsp; 이번 포스트에서는 Datapane 라이브러리를 테스트하고, 사용법을 정리해 보도록 하겠다.

<br/><br/>

## 1. Datapane이란
&nbsp; [Datapane](https://datapane.com/)은 interactive한 report를 쉽게 작성할 수 있는 파이썬 라이브러리이다.

![Datapane example](/images/7th/datapane_example1.jpeg)

&nbsp; 위와 같이 chart 및 table 등을 조합하여 report html 파일을 작성할 수 있다.

<br/>

## 2. Datapane 설치 및 테스트

먼저 Datapane을 설치하자. pip를 이용해 쉽게 설치할 수 있다. 2022/09/21 현재 Datapane 최신 버전인 0.15.1에 약간 문제가 있어 0.14.0 버전을 설치하였다.

```bash
pip install datapane==0.14.0
```

잘 설치됐는지 확인해보자.

```python
import datapane
datapane.__version__

'0.14.0'
```

&nbsp; 기본적으로 Datapane에서는 기존 Object들을 Wrapper 클래스로 둘러싸고, 마지막에 Report 클래스로 요소들을 종합하는 형식으로 활용한다. 아래 예시는 간단한 텍스트를 포함한 Report를 작성하는 코드이다.

```bash
import datapane as dp

text = dp.Text("Hello, World!")
report = dp.Report(text)
report.save("hello.html")
```
위 코드를 실행하면 ```hello.html``` 이라는 파일이 생성된다.
![Datapane example](/images/7th/datapane_hello.jpeg)

<br/>

## 3. Datapane Block

&nbsp; Datapane에서는 Python Object들을 감싸는 Wrapper 클래스를 Block이라고 부른다. Report는 여러 Block들의 조합으로 구성할 수 있다. Block들의 종류는 많기도 하고, 계속 추가되고 있기 때문에 자세한 내용은 [Datapane Docs](https://docs.datapane.com/catalogues/blocks/)를 확인하고, 여기에서는 간단하게 몇몇 개만 살펴보자.

- Text Block
&nbsp; 위 예시에서 사용한 것이 Text Block이다. 단순 String 만이 아닌, md 파일 형식과 같은 Multi-line Text도 Block으로 만들 수 있다.

```python
import datapane as dp

md = """
안녕하세요. 반갑습니다. *강조*

# 제목

- 요소

"""

report = dp.Report(md)
report.save("multi-line.html")
```

## 4. 임시 유저 삭제하기

&nbsp; 이제 wsl을 재부팅하자.

```bash
// window powershell
wsl --shutdown
wsl
```

&nbsp; 변경된 유저로 접속이 되는 것을 확인할 수 있다. 이제 임시 유저를 삭제하자.

```bash
// ubuntu
sudo deluser <임시 유저> // 유저 삭제
sudo rm -r /home/<임시 유저> // 홈 디렉토리 삭제
```
&nbsp; 이것으로 유저 이름 변경이 완료되었다.

<br/>

## 마치며

&nbsp; wsl ubuntu에서 유저 이름 변경하는 법을 알아보았다. 약간의 귀찮음이 있었지만, 기존의 ubuntu에서 유저 이름을 변경하는 법과 크게 다르지 않았다. 많은 도움이 되길 바란다.
