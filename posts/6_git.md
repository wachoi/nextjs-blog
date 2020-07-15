---
title: 'git을 공부하자!'
date: '2020-07-14'
---

노트북을 새로 장만했다. 새 노트북으로 내용을 옮기기 위해 깃헙에 파일들을 올리려니 예상치 못한 문제가 발생했다.
SSH 키 때문에 내 깃헙 계정에 push를 할 수가 없었다. 
SSH 키를 새로 만들고, git config를 다시 해보고, 내 깃헙 계정에 SSH 키를 등록하고 별 짓을 다했지만 계속 permission이 없어 deny되었다는 답만 돌아왔다.
반나절을 이것저것 시도해 결국 push를 올리는데에는 성공했지만 어떻게 성공한 것인지 이해하지 못하니 영 찝찝하다.
(추측하기로는 내가 새로 만든 SSH 키가 특정한 디렉토리에서만 유효한 것이거나, 아니면 내가 깃헙 레포지토리를 SSH 키를 이용해 접근하도록 하지 않았던 것 같다)

그래서 git을 공부하기로 했다. 

감사하게도 인터넷에 좋은 자료가 많다.
[git에서 제공하는 한글 문서](https://git-scm.com/book/ko/v2) 중 1 ~ 3장의 내용을 살펴보았다. 1장이 기초, 2장이 주요 명령어, 3장이 branch를 다루고 있어 꼭 살펴봐야할 것 같고, 4장 이후의 내용은 나중에 더 궁금한 점이 생기면 살펴봐도 될 것 같다. 

1. 시작하기

어떤 큰 작업을 하다보면, 확신이 없는 상태에서 시도를 해보거나 이전 상태로 되돌리고 싶은 경우가 많다. 이때 **버전관리**가 필요하다. 특히 여러 사람이 하나의 작업을 같이 진행할 때에는 어떤 사람이 어떤 작업을 했는지 파악하고, 그것을 받아들일지 말지 결정하는 과정도 필요하다. Git은 이 일을 가능하게 하기 위해 만들어졌다. 

Git은 Linux 커널이라는 매우 큰 프로젝트의 버전관리를 위해, Linux의 창시자인 Linus와 그의 개발 커뮤니티가 같이 만든 **'분산 버전 관리 시스템'**이다. Git이 '분산' 버전관리라고 불리는 이유는 git의 클라이언트들이 마지막 파일의 스냅샷을 가지고 있는 것이 아니라, **저장소와 히스토리를 통째로 복제해 가지고 있기 때문**이다. Git은 여러 개의 리모트 저장소를 가지고 있을 수 있어, 여러 사람과 협업이 가능하고, 데이터도 더 안전하게 보관할 수 있다. 내가 사용하고 있는 Github은 나의 git이 가진 하나의 리모트 저장소이다. 

다른 VCS들은 대개 파일들과 변경사항(델타)을 시간순으로 관리한다. 하지만 Git은 데이터를 스냅샷의 스트림으로 취급한다. 동시에 git의 스냅샷들은 모든 클라이언트들에 통째로 복제되어 있기 때문에, git은 거의 모든 명령을 로컬에서 실행한다. 그래서 매우 빠르고, 네트워크에 연결되어 있지 않아도 작업을 할 수 있다. 로컬에서 commit을 해두면 리모트 저장소에 네트워크로 연결되어 있지 않아도 commit한 내용을 잃어버리지 않는다. 

Git은 파일을 **commited, modified, staged**의 세 가지 상태로 관리한다. 
한 로컬에서 작업을 할 때에는 '본체'라고 할 수 있는 git directory에서 clone하여 로컬에 working directory를 만들고, 거기에서 작업을 하게 된다. 작업을 하면 파일은 modified 상태가 된다. 이 파일을 이제 commit을 할 것이라고 staging area에 올리면 staged 된 상태가 된다. Staged 상태의 파일을 commit하면 영구적인 스냅샷으로 박제가 되어 git directory에 저장된다. 

다시 설명해보자. 내가 이 글을 쓰고 ctrl + s를 누르면 이 md 파일은 modified 상태가 된다. VS code의 source control 창에 보면 'changed' 창에 이 파일이 보인다. 이제 'changed' 창에서 +를 클릭하거나, terminal에서 git add를 하면 파일이 'staged changes' 창으로 이동한다. Staged 상태가 된 것이다. 이제 내가 git command + enter를 입력하거나 commit을 terminal에 입력하면 repository에 commit될 것이다. 


2. Git의 기초

git에서 자주 사용하는 명령들을 알아보자.

* git init: git 저장소에 필요한 skeleton이 들어있는 .git directory를 만들어, 지금 위치하고 있는 디렉토리를 git repository로 만들어준다. 
* git add: git이 그 파일을 track하도록 한다. 그 파일의 그 시점의 스냅샷이 staged area에 올라간다. 
* git cimmit: staged area에 있는 파일들을 스냅샷으로 박제해 git directory에 저장한다. 
* git clone: git repository를 복제한다. git clone 뒤에 url을 입력하면 그 url의 repository를 로컬로 복제해올 수 있다. 
* git status: 내 파일들의 상태를 알려준다. 어떤 branch에 있는지, tracked되고 있는 파일은 무엇인지, staged area에 있는 파일은 무엇인지 등.
* .gitignore: 이건 명령이 아니라, Git repository에서 무시할 파일들의 형식을 지정해둔 파일이다. 터미널에 cat .gitignore 라고 입력하면 어떤 파일들이 무시되고 있는지 확인할 수 있다. Git ignore에 들어갈 파일 형식을 잘 지정해두면, archive나 log 등 저장해둘 필요가 없는 파일들이 따라 올라가지는 않는지 신경쓰지 않아도 된다.
* git diff: Modified, staged, commited 각각의 상태의 파일들 사이의 차이점들을 보여준다. Github에서 훨씬 쉽게 보여줘서 쓸 일이 별로 없을 것 같다. 
* git commit의 --amend 옵션: 바로 직전에 commit한 commit의 내용을 수정할 수 있다. Ammed한 commit은 직전의 commit과 하나의 commit으로 취급된다. 
* git reset HEAD *file*...: Staged area에서 특정한 파일을 내릴 수 있다(unstage). 
* git checkout -- *file*...: Working directory에서 modify한 내용을 버릴 수 있다. 아마도 마음이 아파서 쓸 일이 없을 것 같다..
* git remote: 내가 접속하고 있는 remote server를 보여준다. -v 옵션을 걸면 url을 볼 수 있다. 
* git remote add *shortname url*: 새로운 remote repository에 연결한다. *shortname*에 repository를 부를 이름을 넣고, *url*에 github 등의 url을 넣으면 된다. 참고로, git repository를 clone해 올 때는 remote repository의 이름이 자동으로 origin이 된다고 한다.
* git remote rename *old shortname* *new shortname*: remote repository의 별명을 바꾼다.
* git fetch *shortname*: Remote repository로부터 내 로컬 repository에 없는 데이터를 가져온다. 
* git push *remote branch*: 로컬에서 작업이 마무리되어 remote repository에 공유할 때는 'push'를 한다. 지금까지 아무것도 모르고 'origin master'라고 하면 되는 줄 알았는데, origin이 remote repository의 이름이고, master는 branch의 이름이었다. 이제 잘 구분해서 써야지.
* git remote show *remote*: 특정한 remote repository의 정보를 확인한다.
* git remote remove *shortname*: 특정한 remote repository로의 연결을 끊는다. Reference를 삭제한다는게 좀더 정확한 표현인 것 같다. 어떤 프로젝트에 더 이상 참여하지 않거나, 서버를 옮겼을 때 쓸 수 있다. 
* git tag: Git이 버전관리를 위한 도구인만큼, 버전 표시를 위해 tagging이 필요하다. 아무 옵션도 걸지 않으면 lightweight tag를 만든다. 이 태그는 'v1.8.5'와 같이 이름만 가지고 있다. -a 옵션은 annotated tag를 만든다. 이 태그는 체크섬, 태그를 건 사람의 이름 및 이메일, 생성날짜, 태그 메세지를 포함하고, GNU Privacy Guard (GPG)를 사용할 수 있다고 한다. Annotated tag가 뭔가 좋아보이는데, 아직은 어떤 의미가 있는지 잘 모르겠다.


3. branch의 기초

