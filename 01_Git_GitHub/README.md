# 🐙 Git & GitHub 학습 정리
## 1. Git?
Git은 **파일의 변경 사항을 기록하고 관리하는 버전 관리 시스템**입니다.
예를 들어 프로젝트를 수정하다가 문제가 발생했을 때 이전 버전으로 돌아갈 수 있습니다.
Git은 이런 변경 기록을 관리합니다.

## 2. GitHub?
역할
- 코드 저장
- 프로젝트 관리
- 다른 사람과 협업
- 학습 기록 관리
- 포트폴리오 제작

## 3. Git과 GitHub 차이
| 구분    | Git    | GitHub     |
| ----- | ------ | ---------- |
| 역할    | 버전 관리  | 코드 저장 및 공유 |
| 위치    | 내 컴퓨터  | 인터넷        |
| 주요 기능 | 변경 기록  | 프로젝트 공유    |
| 예시    | commit | push       |

## 4. Repository: Repository(저장소)는 프로젝트와 파일을 저장하는 공간입니다 
## 5. Git 기본 작업 흐름
파일 수정 → git add → staging Area → git commit → Local Repository → git push → GitHub Repository
## 6. git status: 현재 Git파일 상태를 확인합니다
파일을 수정하면 Git은 어떤 파일이 변경되었는지 보여줍니다. 
## 7. git add: 변경한 파일을 Staging Area에 추가합니다
Ex) git add [파일이름], 모든 파일을 추가하려면 git add . 
파일 수정 → git add → Staging Area
## 8. git commit : Staging Area에 있는 변경 내용을 하나의 버전으로 저장합니다
Ex) git commit -m "메시지"
## 9. git push: 내 컴퓨터에 저장된 commit을 GitHub에 업로드합니다
파일 수정 → git add → git commit → git push → GitHub
## 10. git pull: GitHub에 있는 최신 내용을 내 컴퓨터로 가져옵니다 
GitHub → git pull → 내 컴퓨터
## 11. git clone: GitHub에 있는 Repository를 처음으로 내 컴퓨터에 복사합니다
git clone Repository_URL,  Ex) git clone https://github.com/username/LLM_study.git

📌 12. 자주 사용하는 Git 명령어
### 명령어	설명
- git status	현재 파일 상태 확인
- git add 파일명	특정 파일 Staging
- git add .	모든 변경 파일 Staging
- git commit -m "메시지"	변경 내용 저장
- git push	GitHub에 업로드
- git pull	GitHub 최신 내용 가져오기
- git clone URL	Repository 복사

# 실제 학습 기록 방법
1. 공부
2. 폴더 또는 파일 생성
3. 학습 내용 정리
4. git add .
5. git commit -m "학습 내용"
6. git push
7. GitHub에 학습 기록 저장

# 핵심 정리
- Git → 파일 변경 내용을 기록하고 버전을 관리
- GitHub → Git 프로젝트를 인터넷에 저장하고 공유
- git add → 변경 파일을 저장할 준비
- git commit → 변경 내용을 하나의 버전으로 저장
- git push → GitHub에 업로드
- git pull → GitHub의 최신 내용을 가져오기

## 한 줄 정리
Git은 내 컴퓨터에 버전을 관리하고, Github는 그 프로젝트를 온라인에 저장하고 공유하는 공간이다. 
