# git-theory
> 지금까지 어느정도 git의 버전관리 관련 명령어와 브랜치 명령어들과 이론 정도는 알고 있었지만 재대로 개념을 다시 잡을 필요성을 느끼게되 공부한 내용들을 정리하려고 한다. https://seonkyukim.github.io/gitTutorial/index/과  https://subicura.com/git/ 의 내용을 공부후 파트별로 정리하려고 한다.

> 1. git의 3가지 목적
  * git은 크게 3가지의 목적으로 많이 사용된다.
    1) 버전관리 : commit명령을 통해 git 자체에서 커밋당시의 내용을 하나의 버전으로 만들어준다
    2) 백업 : 원격저장소(github)를 통해 자신의 파일이나 중요자료를 백업할 수 있고 어떤 컴퓨터에서나 가져와 사용할 수 있다.
    3) 협업 : git의 branch기능과 전략, 깃허브를 통하여 다른 개발자들과 협업이 가능하다.
  
> 2. git 버전관리 명령어
  > git의 버전관리에 관한 명령어는 아래와 같다
    * git init : 버전관리를 원하는 디렉토리를 깃이 관리하게 한다 => git init 명령어는 해당 디렉토리를 깃의 관리하에 두는 명령어이다. 이렇게 깃의 관리하에 들어간 디렉토리를 로컬 저장소(Local Repository)라고 한다.    
    * git add : staging area에 디렉토리의 파일들중 어떤것을 관리할 것인지 정해 올려줘야 깃에서 파일들을 관리 할 수 있는데 이때 staging area에 관리대상 파일을 올려주는 명령어가 add 명령어 이다.
    * git status : staging area에 올라온 파일들의 상태를 확인하는 명령어 이다.
    * git commit -m "커밋 메세지" : 로컬 저장소에 해당 파일들의 상태를 저장한 버전을 생성하는 명령어이다. 
    <img src="https://media.vlpt.us/images/everydamnday/post/e4a54a34-47f6-443b-b46a-46a19d46db98/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-05-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%203.13.01.png"/>
    * 위 사진과 같이 add,commit을 활용해 로컬 저장소에서 디렉토리를 관리해 버전을 만들고 원격 저장소(remote Repository)에 파일을 올리는 경우 push를 사용해서 올려준다. 
    * git에는 크게 3가지 공간이 있는데 작업을 하고 있는 로컬 컴퓨터의 공간인 Working Directory(WD), 커밋할 파일에 대한 정보를 저장하는 staging area(SA) , 스테이징 에리어의 파일들을 커밋하여 영구적인 버전을 저장하는 Repository(git directory) 가 있다.
    * git add는 현재 Working Directory의 내용을 staging area에 올리는데 이때 파일의 이동이 아닌 복사의 개념으로 이해 해야 한다.즉, wd 의 파일을 수정해 sa파일에는 반영되지 않는다.
    * git commit은 sa의 파일을 가지고 최종 Repository에 저장하여 영구적인 스냅샷으로 저장 = 버전을 저장한다.
    * git commit -a -m "--" : -a 를 붙히면 add의 과정을 거치치 않고 바로 커밋가능하지만 한번 커밋했던 파일에만 가능하고 사용에 주의가 필요한 명령이다.
   
    ```
    min@localhost MINGW64 /d/gitProject/gitsubicura
    $ touch red orange

    min@localhost MINGW64 /d/gitProject/gitsubicura
    $ ls
    orange  red

    min@localhost MINGW64 /d/gitProject/gitsubicura
    $ git init
    Initialized empty Git repository in D:/gitProject/gitsubicura/.git/

    min@localhost MINGW64 /d/gitProject/gitsubicura (master)
    $ echo "빨강" >> red

    min@localhost MINGW64 /d/gitProject/gitsubicura (master)
    $ echo "주황" >> orange
    ```
   * 이렇게 로컬 저장소가 되면 해당 디렉토리에 숨김 파일로 .git이라는 파일이 생성되는데 이 파일은 로컬 저장소의 버전정보를 .git에 저장되기때문에 .git 파일은 되도록 지우면 안된다.
 > 3. git log,status
   * 지금까지의 commit 내역을 git log 명령어를 이용하여 확인할 수 있다. 
    ```
        $ git log
    commit d90e77cdcd6a9567bd0b8bd6fbe6161ba25e6802 (HEAD -> master)
    Author: minyoung <urisegea@naver.com>
    Date:   Thu Jul 22 15:21:16 2021 +0900

        v2 commit

    commit 67738e8b8e461f4e145e577f3fb961bfd807c8a6
    Author: minyoung <urisegea@naver.com>
    Date:   Thu Jul 22 15:20:20 2021 +0900

        v1 commit
   ```
   * 위의 red,orange 파일을 add후 커밋한것이 v1, yellow 파일을 추가하여 커밋한것이 v2이다. 이처럼 git log는 커밋된 버전들의 정보를 확인하는 명령어이다.
   * 가장 최근 커밋부터 내림차순이다. 각 커밋의 암호화된 체크섬(id라고 봐도 무방), 저자정보, 커밋날짜 커밋메세지등을 보여준다.
   * git log --patch 명령어로 커밋간의 자세한 차이를 확인할 수도 있다.
   
   * git에 의해 관리되는 파일들의 상태를 확인하는 명령어는 git status이다
   <img src="https://seonkyukim.github.io/assets/images/2019-02-24-git-status/04.png"/>
   * **wd의 파일은 먼저 크게 Untracked, Tracked 두 상태로 나뉘게 된다**. 파일에 수정이 일어나면 git이 파일 변경을 감지하여 사용자에게 알려주는 것과 같이 파일을 추적하는 상태를 **Tracked 상태**, 반대로 파일을 저장소에 저장할 필요가 없이 git이 신경쓰지 않아도 되는 상태를 **Untracked 상태**라고 한다.
   * 파일을 디렉토리에 새로만들경우 해당 파일은 Untracked 상태이고 add 명령어가 적용된 파일은 tracked 상태가 된다.
   * 이 Tracked 상태의 파일들은 또 Unmodified, Modified, Staged 3가지의 상태로 나뉜다. sa에 있는 파일들의 상태는 Staged이다. sa에 있는 파일들을 커밋하게 되면 해당 파일들은 하나의 커밋으로 저장된후, 파일의 상태는 Unmodified로 내려오게 된다. Unmodified 상태의 파일들을 수정하게 되면 Modified 상태가 된다. 이후 다시 git add 명령어를 이요하여 Staged 상태로 올려준 후 커밋을 하는 과정을 반복하게 된다
   * 하나의 파일이 두 개의 상태가 가능 한 
 
