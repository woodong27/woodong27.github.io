---
layout: post
title: "[Github Blog] Github 블로그 만들기 2"
date: 2024-05-08 17:25 +09:00
categories: tech github_blog
tags:
    [
        tech,
        github,
        blog,
        ruby,
        jekyll
    ]
---

>원하는 jekyll 테마를 적용하고, Github Actions를 통해 블로그를 배포한다.

앞선 포스팅에서 Github에서 레포지토리를 생성하고 `Ruby`, `jekyll`, `bundler`를 설치하였다.   
이어서 `jekyll`로 블로그를 생성하고 원하는 테마를 적용하고 Github Actions를 통해 배포하는 과정을 정리한다.

블로그 생성에 앞서, 이전에 만든 레포지토리를 로컬에 클론하고 해당 디렉토리에서 VSCode를 실행한다.

## 1. Jekyll 사이트 생성

VSCode에서 터미널을 키고 아래 명령을 실행한다.   
이전에 jekyll과 bundler를 wsl에 설치했기 때문에, 명령어의 앞에 wsl을 붙이거나 wsl에서 명령을 실행시켜야 한다.

```bash
# wsl 시작 명령
wsl  

# 이런식으로 터미널이 변경되었다면 wsl이 실행되었다.
woodong@DESKTOP-557HK2N:/mnt/c/Users/user/Desktop/Blog$ 

# wsl을 시작했다면 이후 이어질 명령에서 wsl은 생략해도 된다.
# Windows에서 한다면 명령어를 그대로 사용한다.

# 현재 위치한 디렉토리에 jekyll 사이트 생성
wsl jekyll new ./

# bundler로 종속성 설치
wsl bundle install

# Bundler PermissionError가 발생하면 sudo로 실행해본다.
Bundler::PermissionError: There was an error while trying to write to `/var/lib/gems/3.0.0/cache/jekyll-feed-0.17.0.gem`. It is likely that you need to grant write permissions for that path.

wsl sudo bundle install

# 로컬 서버로 jekyll 실행
# 뒤에 --livereload 옵션을 붙이면 변경사항이 발생할 시 자동으로 새로고침 된다.
wsl bundle exec jekyll serve
```

로컬 서버에서 `jekyll` 서버를 실행하면 4000포트에서 사이트를 확인할 수 있다.

```bash
  Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```

<img class="post-image" src="/assets/posts/tech/github_blog/github_blog_2/jekyll_new.png" alt="New Jekyll Blog">

해당 주소로 접속하면 **Welcome to Jekyll!**을 확인할 수 있다.   
이후, 새로운 포스트를 작성하고 배포 전에 로컬에서 jekyll 서버를 실행하여 확인이 가능하다.   

사이트 확인 후에는 `Ctrl + c` 커맨드로 서버를 종료한다.

## 2. Jekyll 테마 선택

`Jekyll` 사이트의 테마를 검색하고 미리 확인할 수 있는 다양한 사이트가 존재한다.   
그 중 [Jekyll themes](http://jekyllthemes.org/)에서 현재 사용하고 있는 테마를 다운로드 받았다.

<img class="post-image" alt="Jekyll themes" src="/assets/posts/tech/github_blog/github_blog_2/jekyll_themes.png">

<img class="post-image" alt="Download theme" src="/assets/posts/tech/github_blog/github_blog_2/select_theme.png">

사이트에서 원하는 테마를 찾았다면 다운로드 버튼을 눌러 zip파일을 다운 받는다.   
다운로드 받은 파일의 압축을 풀고 폴더안의 모든 내용을 복사한다.   
복사한 것을 Github에서 클론 받은 루트 디렉토리에 붙여넣는다.   

이후, 블로그 초기화를 위해 아래의 파일들을 삭제한다.   
아래 목록 중 존재하지 않는 파일이 있다면 해당 파일은 넘어간다.

* `Gemfile.lock`
* `.travis.tml`
* `_posts/` 디렉토리 내부의 파일들
* `docs/` 디렉토리(폴더)

`_posts` 디렉토리에는 블로그에 게시할 포스트들이 저장되며, 처음에는 chirpy 개발자가 커스텀한 임시 포스트들이 저장되어 있다.
{: .notice--info}

파일/디렉토리 삭제 후에는 Github Actions 설정을 위해 `.github/` 디렉토리 내부의 파일을 삭제하고 수정한다.
<img class="post-image" src="/assets/posts/tech/github_blog/github_blog_2/workflow_setting.png" alt=".github 설정">
chirpy 테마를 기준으로 `.github/workflows/starter/pages-deploy.yml` 파일을 제외하고 모두 삭제한다.   

chirpy 테마와 디렉토리 구조나 파일명이 다르더라도,   
`pages-deploy.yml.hook` 파일을 제외한 나머지를 삭제하고 해당 파일의 이름을 `pages-deploy.yml`로 변경한다.

파일 삭제 및 수정이 끝나면 아래 명령어 입력을 통해 테마 적용을 완료한다.

```bash
wsl bundle install
```

## 3. 블로그 설정

`Jekyll` 테마 적용 후에는 나에게 맞게 블로그의 설정을 변경한다.   
설정은 루트 디렉토리의 `_config.yml` 파일의 수정을 통해 변경할 수 있다.

아래에 적힌 항목들을 찾아서 수정한다.   
`_data/locales/` 에서 사용가능한 언어 목록을 확인할 수 있다.

```yaml
lang: en  # 언어 설정

timezone: Asia/Seoul  # 시간대 설정

title: 블로그 타이틀

tagline: 블로그 서브 타이틀  

description: 블로그 소개

url: "https://{github username}.github.io"

github:
  username: github username

social:
  name: 이름
  email: 이메일
  links:
    - 깃헙 주소
    - 기타 sns 주소

# cdn은 앞에 #을 추가해서 주석처리한다.
# cdn: "https://chirpy-img.netlify.app"

avatar: 블로그 아바타 이미지 주소
# 보통 아바타는 assets 디렉토리에 저장한다.
# 사용 방법은 "/assets/img/profile.jpg"와 같이 상대경로로 이미지를 지정한다.

pagenate: 10  # 한 페이지에 보여줄 게시글 수
```

설정이 완료되면 다시 jekyll 서버를 실행해서 변경사항이 적용되었는지 확인한다.

```bash
# 서버 실행
wsl bundle exec jekyll serve
```

블로그의 초기 설정을 완료하고 나머지 설정들은 필요에 따라서 내용을 추가하면 된다.

## 4. 블로그 배포

설정 완료 후 로컬에서 확인까지 끝났다면 Github Actions를 통해 빌드/배포를 진행한다.   
현재 레포지토리의 변경사항을 모두 push하면 자동으로 실행되며, Actions에서 진행상황을 확인할 수 있다.
<img class="post-image" src="/assets/posts/tech/github_blog/github_blog_2/github_actions.png" alt="Github Actions">
빌드/배포에서 Fail이 발생하면 해당 항목을 클릭해서 디버깅할 수 있다.

## 5. 정리

Github 블로그에 원하는 테마를 적용하고 Github Actions를 통해 배포하였다.   
이어지는 글에서는 포스트 작성, 댓글 설정, 검색 엔진 등록 등 블로그를 활용하는 방법들을 정리할 예정이다.
