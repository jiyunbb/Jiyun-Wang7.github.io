---
author: Jiyun Wang
layout: post
title: python pyenv virtualenv 가상환경
tags: [python, env]
---

### 가상환경 생성 Python 버전 지정
- pyenv virtualenv [python-version] [virtual_이름]
 
### 가상환경 생성 현재 python 환경으로
- pyenv virtualenv [이름]
 
### 가상환경 사용
- pyenv shell [이름]
 
### 가상환경 목록
- pyenv virtualenvs
 
### 가상환경 활성화/비활성화 - 평소에 안써도 되는데 가끔 꼭 필요할 때가 있음
- pyenv activate [이름]
- pyenv deactivate
 
### 가상환경 삭제
- pyenv uninstall [이름]

### 참고
---
[http://kwonnam.pe.kr/wiki/python/pyenv](http://kwonnam.pe.kr/wiki/python/pyenv)
