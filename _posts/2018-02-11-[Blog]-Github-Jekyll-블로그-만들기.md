## Github-Jekyll 블로그를 선택한 이유

회사에서 바쁘게 개발을 하다보면 중간중간에 내용을 기록하고 싶을 때가 있다. 어렵게 검색해서 정보를 찾았거나, 직접 짠 코드가 마음에 들었거나, 등등 간단히 메모하기는 그렇고 해서 티스토리 블로그를 이용했었다. 내 글쓰는 과정은 다음과 같았다.

### 티스토리 블로그로 글 쓰기

1. 마크다운 에디터를 통해 틈틈히 글을 쓴다.
2. 하나씩 HTML 형식으로 복사해서 티스토리 블로그에 올린다.
3. 코드가 삽입되었을 경우 Syntax-Highlighter 플러그인이 동작하도록 클래스명을 붙여준다.

간단하긴 하면서도 뭔가 마음에 들지 않았고, 오늘 티스토리 블로그 메뉴를 한 번 정리하다가 결국 다시 Github-Jekyll 블로그를 사용하기로 결심했다. 그 이유는 글쓰는 과정이 아래와 같기 때문이다.

### Github-Jekyll 블로그로 글 쓰기

1. 마크다운 에디터를 통해 틈틈히 글을 쓴다.
2. 커밋한다. 모든 글이 등록되었다.

물론 티스토리가 제공해주는 여러 가지 서비스들이 아쉽기는 하다. 하지만 일단 내가 글을 편하게 쓰는게 더 우선인 것 같다. 회사에서 티스토리 블로그를 켜서 글을 옮겨적는 것도 좀 눈치보인다. 회사 컴퓨터든 내 노트북이든  `Typora` 에디터로 깔끔하게 글을 쓴 다음 커밋 명령어로 간단하게 글을 올리고 싶었다.

## Github-Jekyll 블로그 만들기

[깔끔하게 잘 정리된 블로그 글](https://nolboo.kim/blog/2013/10/15/free-blog-with-github-jekyll/)을 찾아서 그대로 따라했다. 지난 번에 적용할 때는 많이 버벅였던 것 같은데 이번에는 두 번의 간단한 트러블슈팅 후 블로그가 개설되었다.

> 작업 환경: MacOS (Sierra)

### 1. Ruby 설치

기본으로 Ruby가 설치되어 있지만, 버전이 2.1이상이 필요한데 2.0이었다. 터미널을 사용해서 업그레이드했다.

```bash
# Ruby 버전 관리 시스템? RVM 설치
$ curl -L https://get.rvm.io | bash -s stable

# 사용가능한 버전 확인
$ rvm list known

# Ruby 버전 선택해서 설치
$ rvm install ruby-2.4.0

# Ruby 버전 확인
$ ruby --version
# > ruby 2.4.0p0 (2016-12-24 revision 57164) [x86_64-darwin16]
```

### 2. Jekyll 설치

링크에서는 jekyll만 설치하라고 되어있는데, 설치 과정을 따라가는 도중 bundler도 필요하다는 메세지가 나타나서 같이 설치해주었다.

```bash
$ sudo gem install jekyll bundler
```

### 3. 로컬에서 블로그 생성

아래 명령어를 입력하면 `USERNAME.github.io`로 디렉토리가 생성된다. 그리고 블로그에 필요한 기본적인 설치도 완료된다.

```bash
$ jekyll new USERNAME.github.io
```

해당 폴더로 이동해서 `$ jekyll serve --watch` 명령어를 입력하면 `localhost:4000` URL에서 블로그를 확인할 수 있다.

### 4. Github와 연동l

이제 외부로 서비스하기 위해 Github와 연동하면 된다. 우선 Github에서 자신의 계정으로 로그인한 후 `USERNAME.github.io`라는 이름으로 프로젝트를 생성해준 후, 아래 명령어로 연동한다.

```bash
# USERNAME.github.io 폴더에서 수행한다.
$ git init
$ git remote add origin 저장소URL
# ex) https://github.com/USERNAME/USERNAME.github.io.git
$ git pull origin master --allow-unrelated-histories
# clone을 하지 않았기 때문에 history 관련 오류가 뜨는 것 같다. 이를 해결하기 위한 옵션이다.
$ git add .
$ git commit -m "Intialize Blog"
$ git push origin master
```

위 작업 완료 후 `USERNAME.github.io` URL로 이동하면 기본 설정으로 생성된 Jekyll 블로그가 보일 것이다. 만약 계속에서 404 에러가 나타난다면 다음 방법을 이용해보자.

1. Github 사이트에서 `USERNAME.github.io` 프로젝트 > 설정(Settings)에 들어간다.
2. 맨 위 옵션인 저장소 이름(Repository name)을 다른 이름으로 바꿨다가 다시 되돌린다.

다시 한 번 출처: https://nolboo.kim/blog/2013/10/15/free-blog-with-github-jekyll/