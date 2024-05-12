---
layout: post
title: "[GitHub Blog] Github 블로그 만들기 3"
date: 2024-05-11 23:55 +09:00
categories: tech github_blog
tags:
    [
        tech,
        github,
        blog,
        utterances,
        post,
        search_engine,
        google,
        naver
    ]
---

>Github 블로그를 활용하는 방법을 간단하게 정리한다.

앞선 포스팅을 통해 Github 블로그를 생성하고 배포하는 방법을 정리하였다.   
이제 만들어진 블로그에 글을 작성해보고, 다른 사용자들이 댓글을 남길 수 있도록 한다.

## 1. 포스팅 작성

Github 블로그의 포스팅은 `_posts` 디렉토리에 마크다운 파일로 작성한다.   
작성하는 파일의 이름은 `yyyy-MM-dd-제목.md`으로 지정한다.   
각 포스팅의 가장 상단에 title, date, category, tag 등 글이 관련된 정보를 입력한다.

```markdown
---
layout: post
title: "제목"
date: yyyy-MM-dd HH:mm:ss +09:00
categories: 카테고리 세부카테고리
tags:
    [
        tag1,
        tag2,
        tag3,
        ...
    ]
---

본문은 아랫부분에 작성한다.
```

date에는 가장 뒤에 +09:00을 붙여서 KST임을 표시해주었다.

작성 후 Github에 push하면 `.github/workflows`에서 지정한 Github Action이 실행된다.   
빌드 - 배포 과정이 완료되면 잠시 후 블로그에서 변경된 사항을 확인할 수 있다.

push전 글이 제대로 작성되었는지 확인하고 싶다면 로컬에서 서버를 실행해서 미리 볼 수 있다.

```bash
# Jekyll 서버 실행 명령어
wsl bundle exec jekyll serve
```

## 2. 댓글 활성화

Github 블로그에는 disqus, utterances, giscus 세가지 방법으로 댓글 기능을 활성화할 수 있다.   
disqus는 jekyll 사이트에서 많이 사용하는 댓글 기능이지만 광고와 관련된 이슈가 있어 utterances를 선택했다.   

utterances는 github issue 기반의 서비스로 댓글을 작성하면 지정한 github repo에 issue로 등록된다.   
이후, github api를 통해 어떤 글의 댓글인지 확인하고 그 내용을 화면에서 보여준다.   

### 2.1 Utterances 설치

[https://utterances.es](https://utteranc.es/)   
해당 링크로 이동해서 utterances의 설치를 진행한다.

<img class="post-image" src="/assets/posts/tech/github_blog/github_blog_3/utterances_repo.png" alt="Utterances setting1">
가장 먼저 issue 생성을 위한 레포지토리를 지정한다.   
현재 github blog 레포지토리를 사용해도 되고 새로운 레포지토리를 생성해서 지정해도 된다.   
다만 지정하는 레포지토리는 public으로 설정되어 있어야 한다.

<img class="post-image" src="/assets/posts/tech/github_blog/github_blog_3/utterances_issue.png" alt="Utterances setting2">
다음으로는 post와 issue를 매핑하는 방법을 지정한다.   
post의 title, pathname등 다양한 방법이 있다.   
path의 경우 거의 변경될 일이 없다고 생각되어서 나는 path로 지정하였다.

<img class="post-image" src="/assets/posts/tech/github_blog/github_blog_3/utterances_option.png" alt="Utterances setting3">
마지막으로 생성될 issue의 label과 utterances의 theme를 지정한다.   
해당 설정들은 optional로 굳이 하지 않아도 문제는 없다.

<img class="post-image" src="/assets/posts/tech/github_blog/github_blog_3/utterances_apply.png" alt="Utterances setting4">
설정이 완료되면 가장 아래에 지정한대로 댓글 생성을 위한 코드가 생성된다.   
COPY버튼을 눌러 코드를 복사해서 `_layouts/post.html`파일의 가장 아래부분에 붙여넣는다.

### 2.2 Chirpy 테마 사용자용 설정

현재 사용중인 chirpy 테마에서는 `_config.yml`파일에서 댓글 기능의 설정을 지원하고 있다.

```yaml
comments:
  # Global switch for the post comment system. Keeping it empty means disabled.
  provider: utterances  # [disqus | utterances | giscus]
  # The provider options are as follows:
  disqus:
    shortname: # fill with the Disqus shortname. › https://help.disqus.com/en/articles/1717111-what-s-a-shortname
  # utterances settings › https://utteranc.es/
  utterances:
    repo: woodong27/comments  # <gh-username>/<repo>
    issue_term: pathname  # < url | pathname | title | ...>
  # Giscus options › https://giscus.app
  giscus:
    repo: # <gh-username>/<repo>
    repo_id:
    category:
    category_id:
    mapping: # optional, default to 'pathname'
    strict: # optional, default to '0'
    input_position: # optional, default to 'bottom'
    lang: # optional, default to the value of `site.lang`
    reactions_enabled: # optional, default to the value of `1`
```

설정파일의 comments 항목에서 discus, utterances, giscus중 원하는 방법을 선택하고 각 항목을 채워주면 설정이 완료된다.

<img class="post-image" src="/assets/posts/tech/github_blog/github_blog_3/comment_test.png" alt="Test comment">
이후 게시글의 아래쪽에서 댓글을 작성하고 확인할 수 있다.(Github 로그인 필요) 

## 3. 정리

Github 블로그에 글을 작성하고 댓글 기능을 활성하하였다.   
이제 기본적인 블로그의 형태를 갖추었다고 생각된다.   

이어지는 게시글에서는 구글/네이버등의 검색 엔진에서 블로그가 검색될 수 있도록 등록하는 과정을 정리할 예정이다.
