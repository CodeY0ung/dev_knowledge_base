# git

---

```jsx
// 같은 뜻임.
작업 디렉토리
working directory
workingtree
**현재 사용자가 작업 중인 폴더(위치)를 뜻합니다**

// git과 계약을 맺겠다. git이 관리하도록 하겠다.
git init

// --local은 해당 dir 안에서만 해당됨. 컴퓨터 전체에 적용하고 싶으면 --global 하면 됨.
git config --local user.name "lwy"
git config --local user.email "wooyounglee3312@naver.com"
// 설정한 값들이 나옴
git config --local --list

// README.md 파일에 # Git 첫 실습 내용 채워넣으면서 동시에 생성
echo "# Git 첫 실습" > README.md

git status

// 스테이징
git add README.md

// commit 할때마다 메세지 등록해줘야함.
git commit -m "첫 번째 commit"

// 해당 컷밋의 고유 번호가 나옴
git log
git log --oneline // 해시코드 짧게해서 보기 좋게

**// vim 에디터
vim a.txt
i
내용입력
esc
:wq

// 작업 디렉토리 영역 파일들의 내용 변경 내역(차이)
git diff

// 스테이지 영역 파일들의 내용 변경 내역(차이)
git diff --staged

// 28e46db 커밋 당시의 a.txt 파일 내용을 보여줘
git show 28e46db:a.txt

// 스테이징 취소(add를 되돌리는 명령이다)
git restore --staged a.txt

git restore a.txt // 파일의 수정 내용 취소됨.

// 바로 앞 전의 commit 취소(commit)
git reset --soft HEAD~1
git reset --soft HEAD~2 // 2번째 전의 commit 취소

// add, commit 둘 다 동시에 취소(commit + add)
git reset --mixedd HEAD~1

// 작업directory 영역에서 사용자가 작업한 내용까지 삭제(commit + add + 작업내용)
git reset --hard HEAD~1

// foo branch 추가
git branch foo
// foo branch로 변경
git switch foo

// add와 commit을 동시에
git commit -am "message"

// merge는 main 브랜치로 옮겨와서
git merge foo

// 쓸모 없어진 브랜치는 삭제하자
git branch -d foo

패스트포워드 머지**
```

repository → local 덮어쓰기

```jsx
git status
// main 브랜치 명 확인
git branch --show-current
git fetch origin

// main 브랜치 명이 main이 아니라 master 일 수 있음.
git reset --hard origin/main
```

git이 관리하는 곳

- 작업디렉토리
- 스테이지
- 리포지토리

버전도 1.1.1 3가지로 나눠서 관리함

- 큰 틀이 바뀐다? 앞자리
- 기능이 수정이 있다? 둘째자리
- 버그 수정이 있었다? 뒷자리

git bash 명령어

```jsx
// 현재 위치
pwd

ls
// 숨김 dir 도 같이 표시
ls -la

// aaa 폴더 생성
mkdir aaa
// aaa 폴더로 이동
cd aaa
//test.txt라는 파일에 test 파일 이라는 내용을 넣어서 생성
echo "test 파일" > test.txt

// 파일 내용 읽기
cat test.txt

// 파일 복사
cp test.txt test2.txt
// 파일 삭제
rm test2.txt
// 파일명 변경
mv test.txt new_test.txt

// 홈으로 지정된 파일 확인
~
cd ~
cd /c/lwy/git-test

```

---

### 원격 저장소 연결 - https 방식

1. 원격 저장소 생성하기
2. 로컬 저장소 생성하기
3. 로컬에서 파일 작업하기
4. 로컬에서 작업한 파일 커밋하기
5. 원격 저장소와 연결하기
    
    ```jsx
    git remote add origin <원격 저장소 주소>
    git remote -v
    ```
    
6. 원격 저장소에 push하기
    
    ```jsx
    // -u는 업스트림이라고 처음에 연결을 뚫어놓는거임
    git push -u origin main
    
    // 두번째 push부터는 그냥 git push 하면 됨.
    git push
    ```
    
7. 로컬에서 파일 작업하고 커밋하기
8. 원격 저장소에서 push하기
9. 원격 저장소에서 파일 생성하고 커밋하기
10. 로컬 저장소에 pull 하기
    
    ```jsx
    // git fetch + git merge
    git pull
    
    // 이건 개인적으로 알아낸 부분
    git fetch origin
    git diff -HEAD origin/main
    git log..HEAD origin/main
    git merge origin/main
    ```
    

---

### 원격 저장소 연결하기 2 - SSH

1. ssh 키 생성
    
    ```jsx
    cd ~
    ssh-keygen
    cd .ssh
    ls -al
    // .pub 붙어있는 id 복사(id_ed25519.pub)
    clip < <복사한 id>
    ```
    
2. 원격 저장소에 ssh 설정
    
    ```jsx
    대시보드에서 내 아이콘 클릭
    settings
    SSH and GPG keys
    NEW SSH KEY
    이름 설정, 키에는 gitbash에서 clip(복사)한 내용 붙여넣기.
    ```
    
3. 원격 저장소 생성하기
4. 로컬 저장소 생성하기
5. 로컬에서 파일 작업하기
6. 로컬에서 작업한 파일 커밋하기
7. 원격 저장소와 연결하기
    
    ```jsx
    git remote add origin <ssh 원격 저장소 주소>
    git remote -v
    ```
    
8. 원격 저장소에 push하기
    
    ```jsx
    // -u는 업스트림이라고 처음에 연결을 뚫어놓는거임
    git push -u origin main
    
    // 두번째 push부터는 그냥 git push 하면 됨.
    git push
    ```
    
9. 로컬에서 파일 작업하고 커밋하기
10. 원격 저장소에서 push하기
11. 원격 저장소에서 파일 생성하고 커밋하기
12. 로컬 저장소에 pull 하기
    
    ```jsx
    // git fetch + git merge
    git pull
    
    // 이건 개인적으로 알아낸 부분
    git fetch origin
    git diff -HEAD origin/main
    git log..HEAD origin/main
    git merge origin/main
    ```
    

---

## .gitignore

git이 관여하지 않아야 할 파일

```jsx
temp/ // temp 폴더 하위 파일들
*.log // 확장자가 .log 인 파일들
c.txt // c.txt 파일
```

---

## git clone

```jsx
// git init을 하지 않고 하네
// local 저장소에 위치한 상태에서 해야 뒤에 .만 붙이면 된다.
git clone https://github.com/CodeY0ung/test4.git .

// 다른 사람 저장소를 clone할때 사용.
// 팀장이 골격 repository 생성
// 팀원들이 clone 해감
// 팀원들은 git pull origin main 후 각자 branch에서 작업 후 각자 branch에 push
// pull request 작성 및 승인
```