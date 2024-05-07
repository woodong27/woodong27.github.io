---
layout: post
title: "[GitHub Blog] GitHub 블로그 만들기 1"
date: 2024-05-07 22:09 +09:00
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

>GitHub 블로그를 만들기 위한 과정을 정리한다.

개발을 하며 겪는 이슈를 해결하거나 관련 지식을 공부하다보면 다양한 개발 블로그를 만나게 된다.   
`velog`, `tistory` 이외에도 `GitHub`을 활용하는 블로그에 흥미를 가지게 되어 선택하였다.

GitHub 블로그는 `Jekyll`이라는 정적 사이트 생성기와 GitHub Actions를 통해 블로그를 만들고 글을 게시한다.   
마크다운으로 포스트를 작성할 수 있어 사용이 간편하고, 현재 사용 중인 `Chirpy` 테마처럼 다양한 테마를 선택해서 적용할 수 있어 선택하였다.

## 1. GitHub 설정

GitHub 블로그를 만들겠다고 마음먹었다는 것은 기본적으로 GitHub에 대한 이해가 있다고 생각하기 때문에, 관련 절차는 간략하게 정리하고 넘어간다. 

### 1.1 Repository 만들기

<img alt="Create new repository" src="/assets/posts/tech/github_blog/github_blog_1/make_repository.jpg" style="align-text: left;" width="100%" height="100%">

`{my_github_username}.github.io`의 이름을 가진 레포지토리를 생성한다.   
레포지토리는 public으로 생성하였다.   
private으로 생성된 레포지토리는 GitHub Actions로 배포시 매월 제공되는 무료 사용량을 초과하면 요금이 부과된다.
{: .notice}

<img src="/assets/posts/tech/github_blog/github_blog_1/github_settings.jpg" alt="GitHub Settings" style="text-align: left;" width="100%" height ="100%">

`Settings` - `Pages` - `Build and deployment`에서 Source를 `GitHub Actions`로 변경한다.

## 2. Ruby 설치

GitHub 블로그를 만들기 위해서는 앞서 얘기한 것과 같이 정적 사이트를 생성하기 위해 `Jekyll`을 설치해야한다.
`Jekyll`을 설치하기 위해서는 사전에 `Ruby`를 설치해야 한다.

`Ruby` 이후 이어질 `Jekyll`과 `bundler`는 모두 WSL에서 설치된다.
WSL의 설치 및 실행은 [[WSL] Windows에서 Linux 개발 환경 설정](https://woodong27.github.io/posts/WSL/) 포스팅을 참고하면 된다.   

Git bash, CMD, Powershell 등 아무 터미널을 키고 아래의 명령어를 순서대로 입력한다.  
Ubuntu-22.04 버전을 기준으로 진행한다.    
터미널은 Windows에서 실행한다.

```bash
# wsl 진입 명령
wsl

# 시스템을 최신 상태로 업데이트
sudo apt update
sudo apt upgrade

# Ruby 설치
sudo apt install ruby-full

# ruby 설치 확인
ruby -v
```

`-v`명령을 입력했을 때 설치된 ruby의 버전 정보가 출력된다면 완료되었다.

## 3. Jekyll & bundler 설치

`Ruby`의 설치가 완료되었으면 `jekyll`과 실행을 위한 `bundler`를 설치한다.

```bash
# jekyll 설치
sudo gem install jekyll

# jekyll 설치 시 아래와 같은 에러가 발생하면 build-essential을 설치해준다.
To see why this extension failed to compile, please check the mkmf.log which can be found here:

  /var/lib/gems/3.0.0/extensions/x86_64-linux/3.0.0/ffi-1.16.3/mkmf.log

extconf failed, exit code 1

Gem files will remain installed in /var/lib/gems/3.0.0/gems/ffi-1.16.3 for inspection.
Results logged to /var/lib/gems/3.0.0/extensions/x86_64-linux/3.0.0/ffi-1.16.3/gem_make.out

# 에러 발생 시
sudo apt install build-essential

# 이후 다시 jekyll을 설치한다.
sudo gem install jekyll

# jekyll 설치 확인
jekyll -v

# bundler 설치
sudo gem install bundler

# bundler 설치 확인
bundler -v
```

`-v` 명령을 입력했을 때 정상적으로 버전 정보가 출력되면 완료되었다.

## 4. 정리

GitHub 블로그 시작에 앞서 개설을 위한 사전 준비를 완료하였다.   
다음 포스팅에서는 생성한 레포지토리에 원하는 테마의 블로그를 작성하고 실행하는 방법을 정리할 예정이다.

