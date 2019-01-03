---
layout: post
title: Devlog 만들기
subtitle: Jekyll과 Gitpage를 연동하여 나만의 개 발로그 만들기.
date:   2018-12-14 12:00
categories: jekyll, gitpage, devlog
permalink: /archivers/setup-devlog
---


### 1. Jekyll을 사용한 이유
이 전에는 HTML + CSS + Javascript +JQuery 로만 홈페이지를 만들었었다.([HERE!](https://soomin0328.github.io/My-Homepage/))  
그런데 홈페이지에 내가 공부하는 것들을 올리고 싶어졌고, 결국 **코드** 를 올려야 하는 상황이 되었다.
어떤 형식으로 코드를 올리는 것이 좋을지 검색하던 와중에 발견한 것이 바로 <code>Jekyll</code>이다.  
#### 1-1. Jekyll 이란?
<code>Jekyll</code>
이란 Github에서 개발한 사이트 개발 툴이다. 정적 사이트이기 때문에 PHP 언어 등을 이용한 서버 소프트웨어 없이 HTML, CSS 등의 정적파일들 만으로 사용자의 컴퓨터에서 사이트 생성이 가능하다. 따라서 매우 빠르고 가벼운 툴이다. 또한 Jekyll은 <code>Markdown</code>을 지원한다.  
<br/>
### 2. Jekyll 설치 과정
#### 2-1. 설치 준비
- 설치환경: Window10
- 필요한 것들
  - Ruby(Ruby+Devkit)
  - Jeykll
  - Python(이미 설치되어 있으므로 패스~!)
  - rouge(syntax highlighter)

#### 2-2. Ruby 설치
<http://rubyinstaller.org/downloads/>  
여기서 내 시스템과 맞는 Ruby-Devkit을 받아 설치한다.  
(이 때, Add Ruby executables to your PATH에 체크하여 어느 경로에서든 Ruby를 실행 할 수 있게 한다.)  
#### 2-3. Jekyll & rouge 설치
``` terminal
~$ gem install jekyll
~$ gem install rouge
```
#### 2-4. Jekyll 실행
``` terminal
~$ jekyll new test-Jekyll
~$ cd test-Jekyll
~/test-Jekyll$ jekyll serve
```
<http://127.0.0.1:4000/> <- 여기를 들어갔을 때


![Screenshot](/../assets/img/screenshot.png)

이런 화면이 나오면 설치 성공!
### 3. Jekyll Theme
#### 3-1. Theme 적용
<http://jekyllthemes.org/>  
여기서 원하는 Theme의 Github에 들어가서 <code>git clone</code>한다.  


그 다음
``` terminal
~$ gem install bundle
~$ bundle install
~$ jekyll serve
```

 테마가 적용된 것을 확인한다!
#### 3-2. Theme Customizing
Jekyll의 기본 디렉터리 구조는 아래와 같다.
``` terminal
.
├── _config.yml
├── _data
├── _drafts
├── _includes
│   ├── footer.html
│   └── header.html
├── _layouts
│   ├── default.html
│   └── post.html
├── _posts
│   ├── 2007-10-29-why-every-programmer-should-play-nethack.md
│   └── 2009-04-26-barcamp-boston-4-roundup.md
├── _sass
│   ├── _base.scss
│   └── _layout.scss
├── _site
├── .jekyll-metadata
└── index.html
```

각 디렉터리의 자세한 설명은 [여기](https://jekyllrb-ko.github.io/docs/structure/)에서 알 수 있다.


설명을 보고 나면, 내가 테마를 커스터마이징하기 위해 필요한 기본적인 파일들은 다음과 같다>>
<code>_config.yml, _include, _layouts, _sass</code>  


'적고 나니 대부분인데,,?' 라는 생각이 들지만 그래도 바꿔본다!


우선 _config.yml 파일은 **환경설정** 정보를 보관하기 때문에 필요한 정보들을 바꿔줘야 한다.  
``` yml
# SITE CONFIGURATION
baseurl: ""
url: "https://soomin0328.github.io/"
```
baseurl에 아무것도 넣지 않는 것이 핵심이다!
``` yml
theme_settings:
  title: Soom's Devlog
```
개발 블로그 타이틀도 바꿔준다!


이 외에 자신이 설정하고 싶은 것들을 바꿔주면 된다.


<code>_include, _layout, _sass</code>는 그냥 자신이 원하는 레이아웃과 css로 커스터마이징하면 된다.  
#### 3-3. Code Highlighting
내가 Jeykyll을 사용한 이유는 코드를 이쁘게(?) 올리기 위한 것인데, 이 때 필요한 것이 Hightlight 기능이다.  
2-3에서 미리 rouge를 설치했기 때문에 설치는 패스하고 적용 과정으로 넘어가겠다.  
나는 sublime 스타일을 적용했다.
``` terminal
~$ rougify styly monokai.sublime >
assets/css/syntax.css
```
위 커멘드를 입력하면 해당 위치에 <code>syntax.css</code>가 저장된다.  
이를 사이트에 적용하기 위해서는
<code>_layout</code>폴더에 <code><head></code>가 존재하는 파일에다가  
``` html
<head>
    <!-- Document Settings -->
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />

    <!-- syntax.css -->
    <link rel="stylesheet" href="/assets/syntax.css">
</head>
```
위 코드처럼 css를 추가해주면
~~~ markdown
``` html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>

  </body>
</html>
```
~~~
이렇게 ```를 이용하여 해당 언어의 코드를 Highlighting 할 수 있다.
