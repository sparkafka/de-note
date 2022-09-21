---
layout: post
title: "[Python]Datapane을 이용한 보고서 작성"
excerpt: Python Datapane 라이브러리를 사용해 HTML 보고서를 작성하자
categories:
    - Python
last_modified_at: 2022-09-21
published: false
---

## 들어가며

&nbsp; 회사에서 Python을 활용한 보고서 작성 프로그램을 만들어야 했다. 파이썬의 여러가지 라이브러리르 비교한 결과, Datapane이라는 라이브러리를 쓰기로 결정하였다.
이번 포스트에서는 Datapane 라이브러리를 테스트하고, 사용법을 정리해 보도록 하겠다.

<br/><br/>

## 1. Datapane이란
&nbsp; [Datapane](https://datapane.com/)은 interactive한 report를 쉽게 작성할 수 있는 파이썬 라이브러리이다.

![Datapane example](/images/7th/datapane_example1.jpeg)

&nbsp; 위와 같이 chart 및 table 등을 조합하여 report html 파일을 작성할 수 있다.

<br/>

## 2. Datapane 설치

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

<br/>

## 3. 이름 변경하기

&nbsp; 이제 원래 유저 이름을 변경하자.

```bash
// ubuntu
sudo usermod -l <변경할 유저 이름> <원래 유저 이름> // 유저 이름 변경
sudo usermod -d <변경할 유저 이름> -m <변경할 유저 이름> // 홈 디렉토리 변경
```

<br/>

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
