---
layout: post
title: "[etc] wsl에서 유저 이름 바꾸기"
excerpt: wsl에서 우분투 유저 이름을 변경해보자
categories: ['etc']
last_modified_at: 2022-10-25
---

## 들어가며

&nbsp; wsl 계정 이름을 바꿀 일이 생겼는데, wsl에서 ubuntu 유저 이름을 변경하는 법을 정확히 서술한 곳을 찾기 어려워서 약간 애먹었다. 나중에 다시 변경할 일이 있을 지도 모르니 정리해 보았다.

<br/><br/>

## 1. 우분투에서 유저 생성

&nbsp; 유저 이름을 변경하기 위해 먼저 임시 유저를 만들어야 한다.

```bash
// ubuntu
sudo adduser <임시 유저> // 임시 유저 만들기
sudo adduser <임시 유저> sudo // 임시 유저에 su 권한 주기
```

<br/>

## 2. 임시 유저에 로그인 하기

&nbsp; 이제 wsl에서 원래 ubuntu 계정이 아닌, 임시 계정으로 로그인을 해야 한다. Window에서 새로 만든 계정으로 로그인하자.

```bash
// window powershell
wsl --shutdown
wsl -u <임시 유저>
```

&nbsp; 이렇게 접속하면 임시 유저로 로그인이 되는 것을 확인할 수 있다.

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
