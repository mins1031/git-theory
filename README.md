# git-theory
> 지금까지 어느정도 git의 버전관리 관련 명령어와 브랜치 명령어들과 이론 정도는 알고 있었지만 재대로 개념을 다시 잡을 필요성을 느끼게되 공부한 내용들을 정리하려고 한다. https://seonkyukim.github.io/gitTutorial/index/과  https://subicura.com/git/ 의 내용을 공부후 파트별로 정리하려고 한다.

## 1. git의 3가지 목적
  * git은 크게 3가지의 목적으로 많이 사용된다.
    1) 버전관리 : commit명령을 통해 git 자체에서 커밋당시의 내용을 하나의 버전으로 만들어준다
    2) 백업 : 원격저장소(github)를 통해 자신의 파일이나 중요자료를 백업할 수 있고 어떤 컴퓨터에서나 가져와 사용할 수 있다.
    3) 협업 : git의 branch기능과 전략, 깃허브를 통하여 다른 개발자들과 협업이 가능하다.
  
## 2. git 버전관리 명령어
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
  
 ## 3. git log,status

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
   * git checkout --체크섬 : 워킹 디렉토리의 modified 특정 파일을 가장 최근 커밋 버전으로 되돌릴 수 있다. 즉 수정 사항을 제거하는것인데 이 기능은 되돌릴수 없어 주의해서 사용해야 한다.
  ## 4. git reset,revert 
    
   * git reset - 이전 상태로 (이력 제거) 
     * 특정 커밋까지 이력을 초기화 한다. 바로전이나 n번 전까지 작업했던 내용을 취소 할 수 있다.
     * 즉 특정 커밋의 상태가 되고 싶을 때(=특정 커밋의 상태로 돌아가고 싶을 때) 사용한다
     * 다만 기존 이력이 지워질수 있음으로 주의해야 한다.
   ```
   git reset --hard 커밋체크섬
   reset 의 옵션은 많다 --hard는 해당 커밋 위의 내용과 수정사항까지 완전히 지워버리기때문에 특정 커밋의 상태로 완전히 돌아갈 수 있다. 다만 협업을 하고 있고 이미 공유했던 내용에 대해 reset은 하지 않는것이 좋다
   ```
   * git revert - 이전 상태로 (이력 유지) 
     * reset은 버전이 지워지기에 리스크가 따른다. 하지만 revert는 기존 이력을 유지하며 돌아간다는 장점이 있다
     * 만약 v1,v2,v3,v4라는 커밋이 있고 v4가 최근 커밋인 동시 현재 커밋이다. 이경우 v3로 revert하고 싶은경우 v4의 체크섬이 필요하다. revert가 진행되고 log를 보면 v4가 새로 하나 더 생성된것을 알수 있다.
     * 이 생성된 커밋은 v4의 변화된 내용을 취소 했기 때문에 v3라고 할수 있고 v4도 유지한 상태로 v3로 돌아간것과 다름없는 상황이 되었다.(엄연히는 v3자체가 된게 아닌 v3 상태의 커밋을 하나 만든것이다.)
     * 다만 v4의 상태에서 v2를 가려면 v3의 체크섬으로 revert하는것이 아닌 v4로 revert후 v3로 revert하는 내림차순의 형식을 꼭 지켜줘야 한다. 아니면 충돌이 발생한다고 함
     ```
     git revert v4의 체크섬
     ```
 
 ## 5. 리모트 저장소 (remote Repository)
   * 리모트 저장소는 인터넷이나 네트워크 어딘가에 있는 저장소를 말한다. 예를 들어 깃헙에 만든 저장소 역시 리모트 저장소이다
   * 그렇다면 내 로컬 파일을 원하는 저장소에 보내기 위해선 저장소 경로에 파일을 보내라고 git에게 알려주어야 한다
   > git remote (리모트 저장소 확인)
   ```
   git remote
   origin
   ```
   * 리모트 저장소의 이름과 경로를 확인하려면 -v 옵션으로 확인가능하다.
   ```
   $ git remote -v
   origin  https://github.com/mins1031/spring-boot-jpa-mysql-minair.git (fetch)
   origin  https://github.com/mins1031/spring-boot-jpa-mysql-minair.git (push)
   ```
   * git remote add 'remoteName' 'url' : 리모트 저장소 추가하는 명령어이다. 현 디렉토리의 리모트 저장소로이름을 정하고 url을 설정한다
   ```
   git remote add origin https://github.com/mins1031/git-theory
   ```
   * git push : 로컬 디렉토리에 저장된 커밋을 리모트 저장소로 옮기는 명령어가 push이다 반대로 리모트 저장소의 내용을 가져오는것을 pull한다고 한다. 

## 6. branch를 통한 협업
> 브랜치를 이해하기전 커밋간의 관계를 살펴보아야한다. 각각의 커밋은 이전 커밋의 해시정보를 포함하고있다. 커밋은 트리구조로 .git파일에 저장되어 있으며 각 커밋은 이전 커밋의 주소값(해시정보값)을 포함하고 있다.
**위에도 언급했지만 https://seonkyukim.github.io/git-tutorial/git-branch/ 님의 내용을 보고 복기하는 내용이다. 예시가 너무 잘되어있어 참고한다**
<img src="https://seonkyukim.github.io/git-tutorial/git-branch/"/>
* 위의 사진에서 초록 박스는 커밋이다 오른쪽이 제일 최근 커밋이다.
* 위와 같은 모습들이 나무가 가지치는 모습과 같아 브랜치라고 부른다고 한다. 
* 처음생기는 브랜치를 master브랜치라고 한다. master브랜치에서 다른 브랜치들이 뻗어가는 형태가 구성된다.
* 하나의 게시판 api를 구성시 한명은 게시판 CRUD작업을 하고 다른 한명은 로그인 작업을 할때 두명 모두 master브랜치에서 작업할 경우 다른사람의 작업에 영향을 주고 받을 수 있기 때문에 master브랜치에서 각 브랜치를 나눠 개발후 다시 합치는 과정으로 진행한다

<img src="https://seonkyukim.github.io/assets/images/2019-03-01-git-branch/06.png"/>

* 첫 커밋은 맨 왼쪽 초록상자를 master 브랜치가 가르키고 있을 것이고 커밋이 이어져 세번쨰 커밋시 master 브랜치는 맨 오른쪽 초록 상자를 가리킨다. 즉 브랜치는 최신 커밋을 가리키고 있다.

### 브랜치 생성
* 여기서 snu라는 브랜치를 만든다.

```
$ git branch snu

$ git branch
* master
  snu
```

<img src="https://seonkyukim.github.io/assets/images/2019-03-01-git-branch/07.png"/>

* 현재 브랜치를 가리키고 있는 포인터(주소값)을 HEAD라고 한다.

### 브랜치 이동
* git checkout은 브랜치를 이동하는 명령어인데 head를 이동시키는 명령어라고 생각하면 된다.

```
$ git checkout snu
snu 브랜치로 이동하는 명령어
```

<img scr="https://seonkyukim.github.io/assets/images/2019-03-01-git-branch/08.png"/>
* master브랜치에서 snu 브랜치로 왔으므로 head는 snu를 가리키로 있다.

```
$ git checkout -b snu
브랜치 생성과 동시에 생성 브랜치로 이동하는 명령어

$ git checkout -b test 커밋id
예전 커밋(버전)을 기준으로 다른 브랜치를 만들고 싶은 경우 사용한다. 사용할 브랜치명과 기준되는 커밋의 id값을 명령어에 포함해준다.
```

### 브랜치 병합(merge)
* v3 버전 master에서 apple,goole이라는 브랜치로 나눠지면 각각의 용도에 맞는 버전을 나눠주는 훌륭한 기능이 된다
* 이제 각각의 기능인 master,apple,goole중 apple의 내용을 master브랜치에 더하고 싶은 경우가 있따
* 이러한 경우 merge. 브랜치 병합을 사용한다.
* 이때 master,apple,google이 나눠진 분기점이 되는 v3를 base 커밋이라고 하고 base를 바탕으로 merge된 커밋을 merge commit이라고 한다.

```
 $ git log --all --graph --oneline
* 64ca85a (HEAD -> o2) o2 work2
| * cd66bc9 (master) master work 2
|/
* f843ece work 1

 ```
* git log 명령어를 통해 본 위의 로그그래프는 work1 커밋을 base로 master와 o2의 work2 버전이 커밋된것을 볼 수 있다.
* 이제 위의 상태에서 서로 다른 파일이 있는 브랜치를 병합하는경우는?
  * master work2는 master.txt와 work.txt가 있고 o2 는 o2.txt, work.txt가 있다. 
  * 이때 master에 o2 브랜치를 병합하고 싶어지는 경우엔 우선 master 브랜치를 head가 가리키게 하고 현 브랜치로 가져오고 싶은 브랜치를 merge명령을 통해 지정해 가져오면 된다
  ```
  $ merge o2
  명령어를 입력하면 편집툴(nano나 vim)이 나오는데 위쪽에 왜 병합하는지에 대한 메세지를 남길수 있다.
  머지 하게 되면 o2는 그대로 남아있고 master는 o2와 합쳐진 형태로 만들어지게 된다

  $ git log --all --graph --oneline
  *   e456f6a (HEAD -> master) Merge branch 'o2' into master merge 실습
  |\
  | * 64ca85a (o2) o2 work2
  * | cd66bc9 master work 2
  |/
  * f843ece work 1
  master와 o2가 합쳐져(둘을 조상으로 하는) 하나의 새 커밋이 된것을 볼 수 있다.
  ```
* 이번엔 같은 파일에서 다른 부분이 수정된 경우 병합되면 어떻게 될까
  * 똑같이 master와 o2브랜치를 놓고 이 두 브랜치엔 work.txt란 파일이 있다.
  ```
  master의 work.txt는
  #title 
  master content
  
  #title 
  content
  이렇게 있고
  
  o2의 work.txt는
  #title 
  content
  
  #title 
  o2 content
  이런 방식으로 구성되어있다 
  이 둘을 합치게되면 
  
  $ cat work.txt

  # title
  master content

  # title
  o2 content
  이렇게 병합된다.
  ```
  * 위의 내용을 보면 알겠지만 결국 같은 파일을 수정해도 같은 내용을 수정하지 않았지 떄문에 공존할 수 있어 병합을 시켜주었다.

* 마지막으로 이번엔 같은파일의 같은 부분을 수정하는 경우이다.
  * 깃은 이것을 자동으로 수정하지 못하고 '충돌'이 일어나게 된다.
  * 전 경우와 똑같이 master,o2 브랜치와 각 브랜치엔 work.txt파일이 있다
  ```
  master는 
  # title
  content
  master   ===> o2에선 master대신 o2가 적힘
  # title
  content
  이렇게 되면 둘의 같은 부분에서 수정이 일어 낫고 이걸 병합하려고 하면
  
  $ git merge o2
  Auto-merging work.txt
  CONFLICT (content): Merge conflict in work.txt
  Automatic merge failed; fix conflicts and then commit the result.
  위처럼 충돌에러가 발생하게 된다
  ```
  * 그래서 work.txt를 확인해보면 
  ```
  # title
  content
  <<<<<<< HEAD
  master
  =======
  o2
  >>>>>>> o2
  # title
  content
  위처럼 보인다
  가운데 ===는 '구분자'라고 하고 <<<<부터 구분자까진 head 브랜치에서의 내용이고 구분자부터 >>>>까진 o2브랜치의 내용이었다는 것을 표기하고 있다. 
  저 부분은 깃이 자동으로 병합을 하지 못해 개발자에게 수정을 요구하는 표기이다.
  이후 master와 o2내용둘다 하려면
  # title
  content
  master, o2
  # title
  content
  
  위처럼 적용후 git add work.txt를 해주면 깃에게 충돌을 해결했다는 의미의 메세지를 던져준것과 같다
  ```
  
 * 3 way-merge 
 ```
 front   base   back   2 way merge   3 way merge
 a        a      a         a             a
 f        b      b         ?             f
 c        c      k         ?             k
 f        d      k         ?             ?
 
 front와 back은 base로 부터 가지쳐진 브랜치들이고 내용이 바뀌었다.
 2 way merge는 front와 back 두개의 브랜치를 병합시의 결과를 나타낸다 3개가 충돌이 나게 된다
 반면 3 way merge는 front와 back의 조상 버전인 base의 내용과 비교해 충돌여부를 나타낸다
 front의 2번째 f와 base의 b와 front의 b 이렇게 3개를 비교시 back은 base와의 내용을 보았을 때 바뀌지 않았지만 front는 f로 바뀌었음으로 바뀐값을 병합된 값으로 적용하고
 마지막 front의 4번째 f값 base의 d front의 k값을 보았을때 셋다 값이 다르기때문에 어떤 값이 적용되야하는지 모르고 충돌이 발생하게 된다
 ```
 ### 브랜치 merge와 rebase
 **https://firework-ham.tistory.com/12 참고**
 > merge와 rebase는 비슷해보이면서도 차이점을 가지고 있다.
 * merge는 `브랜치를 통합하는 것` 이고 rebase는 `브랜치의 base를 옮긴다는 개념`의 차이정도가 있다
 #### Rebase
 > rebase는 의미 비슷하게 base를 재설정한다는 의미이다.
 * base는 각 브랜치의 base를 의미한다
 * 각 브랜치는 분기되었던 base지점을 가지고 있어 base부터 코드를 수정하게 된다
 <img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcofZo0%2FbtqBkOJybm6%2FRshCv0OqsydpuNCRPcIxu1%2Fimg.png'/>

 * b 지점을 base로 가진 브랜치가 D,E 커밋을 진행
 * C지점으로 base를 이동하기 위해 브랜치에서 C지점으로 rebase를 한다.
 * C지점으로 rebase 되면 기존 D,E 커밋은 새롭게 정렬되어 C지점 이후로 변경된다.
 * rebase로 base를 옮겨서 작업하면 git history가 굉장히 깔끔해진다. 
 
 #### Rebase를 사용했을때 git History의 효과 확인
 > branch1, branch2를 dev 브랜치에서 생성한뒤 각각 1- commit1  2- commit2,3를 했다. 이상태에서 dev브랜치에 branch1, branch2 두개를 dev에 반영해보려 한다.
 1) merge만 사용했을 경우
   1-1) dev에 branch1 머지
   <img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FFutre%2FbtqBhDC9s50%2Fcx5OzkGcWulfBc2Qa8uuIk%2Fimg.png'/>
   
   * dev와 branch1을 머지한 결과 commit1이 dev로 머지되는 것을 볼수 있다
   1-2) dev에 branch2 머지
   <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F8jS5p%2FbtqBjBj2KKF%2FQHGBC5iOYBf555TadOxFH1%2Fimg.png"/>
   
   * 머지만으로 dev에 코드를 추가하면 위와 같이 히스토리가 꼬이게 된다.
   * 개발자가 늘어나면 각 브랜치도 늘어나게 되고 히스토리가 굉장이 복잡해진다
 2) merge와 rebase를 사용하여 dev에 반영
   2-1) dev에 branch1 머지
   <img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FFutre%2FbtqBhDC9s50%2Fcx5OzkGcWulfBc2Qa8uuIk%2Fimg.png'/>
   
   2-2) dev에 branch1 rebase
   * branch1이 merge된 dev 브랜치를 branch2에 rebase하면
   <img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbFl3P2%2FbtqBjCJ6hsW%2F3FEkI3CfSPBPK7l6dTjVVK%2Fimg.png'/>
   
   * rebase를 하게 되면 히스토리를 커밋이 재정렬하고 branch2의 base 자체를 dev로 지정이 된다
 
   2-3) dev와 branch2 머지
   이상태에서 branch2와 dev를 머지하게 된다면
   <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdsZLrw%2FbtqBjUKskkS%2FY3OKpcjkyFJVUzDRFwkRk0%2Fimg.png"/>
   
   * branch2를 dev에 머지하면 위 사진과 같이 깔끔한 git 히스토리를 확인할 수 있다.
   * 개발자가 여러명이더라도 순서대로 커밋한 것과 같은 히스토리 생성이 가능하다.
 
 ### git flow
 <img src="https://s3-ap-northeast-2.amazonaws.com/opentutorials-user-file/module/2676/5263.png"/>
  
 > git flow 모델은 브랜치를 어떻게 운영할 것인가? 를 보여주는 좋은 사례중 하나라고 할 수 있다. 사진에서 볼수 있듯 master와 develop 브랜치, 기능개발시 사용하는 feature 브랜치와 출시시 사용하는 release 브랜치, 마지막으로 긴급한 수정사항을 반영하는 hotfix브랜치가 있다.
 
 * master는 언제나 실행가능한 최신의 버전이어야 한다.
 * 또한 실행가능한 상태를 만들어가는 과정은 develop 브랜치에서 진행해야한다
  
  
