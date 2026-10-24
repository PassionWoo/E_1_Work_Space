# Codyssey Pre-course | 내 컴퓨터에 개발자용 작업실 꾸미기

> **과제:** 입학연수(Pre-course) — 개발 입문 (필수)
> **수행일:** 2026-10-24
> **GitHub:** [PassionWoo/codyssey-precourse](https://github.com/PassionWoo/codyssey-precourse)
> **최초 Commit:** `b4faffb` (`Initial commit: Linux practice and Docker web server`)

## 1. 프로젝트 개요

이 프로젝트는 **Windows + WSL 2(Ubuntu) + Docker Desktop + Git/GitHub + VS Code**를 연결하여 로컬 개발 환경을 구성하고, 해당 환경이 실제로 작동하는지 **명령어와 실행 결과로 검증**한 기록이다.

구체적으로 Linux CLI의 파일·디렉터리 조작과 권한 변경, Docker 이미지 및 컨테이너 관리, Dockerfile을 이용한 커스텀 NGINX 이미지 제작, 포트 매핑, 바인드 마운트, Docker 볼륨의 영속성 검증, Git 버전 관리와 GitHub·VS Code 연동을 수행했다.

### 실행 환경의 구분

```text
Windows (PowerShell · Chrome · VS Code 화면)
    └── WSL 2 / Ubuntu (bash · 프로젝트 파일 · Linux Git)
         ├── Docker CLI ──> Docker Desktop의 Linux 엔진
         │                    ├── hello-world 컨테이너
         │                    ├── Ubuntu 24.04 컨테이너
         │                    └── NGINX 웹 서버 컨테이너
         └── Git(로컬 Commit) ──> GitHub(원격 Repository)
```

**주의:** WSL Ubuntu와 Docker 내부의 Ubuntu 컨테이너는 별개의 환경이다. VS Code에서 WSL로 연결하면 Windows에서 화면을 사용하면서 Ubuntu 내부의 파일과 Git을 직접 조작할 수 있다.

## 2. 실행 환경

아래 값은 실습 당시 확인한 버전이다. 확인하지 않은 항목은 임의로 채우지 않았다.

| 구분 | 실습 환경 / 버전 | 확인 방법 |
|---|---|---|
| 호스트 OS | Windows (세부 에디션·빌드 미기록) | Windows PowerShell 실행 |
| Windows 터미널·셸 | Windows PowerShell (세부 셸 버전 미기록) | `PS C:\...>` 프롬프트 |
| Linux 환경 | WSL 2의 Ubuntu 배포판 | `wsl --list --verbose` |
| Linux 셸 | Ubuntu bash | `pwd`, `whoami`, bash 프롬프트 |
| Docker Desktop | 4.96.1 | `docker info`의 Desktop 플러그인 |
| Docker CLI / Engine | 29.9.2 / 29.9.2 | `docker --version`, `docker info` |
| Docker Linux 엔진 | `desktop-linux`, Linux / x86_64 | `docker info` |
| Git (Windows) | 2.56.0.windows.3 | PowerShell의 `git --version` |
| Git (WSL Ubuntu) | 2.53.0 | bash의 `git --version` |
| VS Code | 1.140.0 | `code --version` |
| Ubuntu **컨테이너** | Ubuntu 24.04.5 LTS | 컨테이너 내부 `cat /etc/os-release` |
| Git 기본 브랜치 | `main` | `git status`, `git log -1 --oneline` |

> Ubuntu **컨테이너**의 24.04.5 버전은 확인했지만, WSL Ubuntu 배포판 자체의 세부 버전은 별도로 기록하지 않았다. 두 환경의 버전을 혼동하지 않는다.

## 3. 수행 체크리스트

- [x] WSL 2 / Ubuntu 및 Docker 설치·엔진 점검
- [x] 터미널 기본 조작: 위치, 숨김 파일 목록, 이동, 생성, 복사, 이름 변경, 삭제, 파일 내용 확인
- [x] 파일 1개 및 디렉터리 1개의 권한 변경 전·후 비교
- [x] `hello-world` 이미지 다운로드 및 컨테이너 실행
- [x] Ubuntu 컨테이너 진입, 종료, 재시작, `attach` 성격의 연결 및 `exec` 비교
- [x] `docker images`, `docker ps -a`, `docker logs`, `docker stats` 등 기본 운영 명령
- [x] Dockerfile 직접 작성, NGINX 기반 커스텀 이미지 빌드·실행 및 별도 이미지 재빌드 검증(8083)
- [x] 8080·8081 호스트 포트 매핑 및 브라우저 접속
- [x] 바인드 마운트: 변경 전 → 변경 후 → 복구 재검증 (8082)
- [x] Docker 볼륨: 컨테이너 삭제 뒤에도 파일이 유지되는지 검증
- [x] Git 사용자 정보·기본 브랜치 설정, `git add`·`commit`·`status`·`log`
- [x] VS Code의 WSL 원격 연결 및 GitHub 인증·비공개 Repository 게시
- [x] 실제 발생한 문제 2건 이상의 트러블슈팅 정리
- [x] `git config --list` 확인 및 개인정보 마스킹한 핵심 설정 증거 저장
- [ ] **최종 제출 전:** 보완된 README 및 증거 파일을 Commit/Push하고 GitHub에서 모든 링크 확인
- [ ] **최종 제출 전:** 평가자에게 저장소 접근 권한 제공(공개 또는 제출 지침에 맞는 권한)

## 4. 저장소 구성

```text
codyssey-precourse/
├── 01_terminal/
│   ├── empty.txt
│   └── hello.txt
├── 02_webserver/
│   ├── Dockerfile
│   └── site/
│       └── index.html
├── evidence/                  # README와 함께 업로드할 증거 자료
│   ├── 501_권한변경.jpg
│   ├── 502_컨테이너_실행.jpg
│   ├── 503_포트매핑_8080.png
│   ├── 504_포트매핑_8081.png
│   ├── 505_바인드마운트_변경전.png
│   ├── 506_바인드마운트_변경후.png
│   ├── 507_VSCode_WSL_Git연결.png
│   ├── 508_GitHub_저장소.png
│   ├── bind-mount-after.html
│   ├── git-config-sanitized.txt
│   ├── 보완_재검증_로그.md
│   └── 실행_검증_로그.md
└── README.md
```

`01_terminal/archive/` 디렉터리는 파일 삭제 후 비어 있어 최초 Git Commit에 포함되지 않았다. Git은 빈 디렉터리만으로는 추적하지 않는다. 재현 과정에서 `mkdir archive`로 생성한다.

실제 명령과 핵심 출력은 아래 본문 및 [실행·검증 로그](evidence/실행_검증_로그.md)에 정리했다.

## 5. Linux CLI 및 권한 실습

**실행 위치:** WSL Ubuntu의 bash. 프로젝트는 Windows 마운트 경로(`/mnt/c/...`)가 아닌 Linux 홈(`/home/...`)에 생성했다. Linux 파일 시스템에서 권한 변경을 검증하기 위한 선택이다.

### 5.1 파일·디렉터리 조작

```bash
cd ~
mkdir -p ~/codyssey-precourse
cd ~/codyssey-precourse
pwd
ls -la

mkdir 01_terminal
cd 01_terminal
touch empty.txt
echo "Hello Linux" > hello.txt
ls -la
cat hello.txt

cp hello.txt hello-copy.txt
ls -l
mv hello-copy.txt hello-renamed.txt
ls -l
mkdir archive
mv hello-renamed.txt archive/
ls -l archive/
cat archive/hello-renamed.txt
rm archive/hello-renamed.txt
ls -la archive/
```

핵심 확인 결과:

```text
$ pwd
/home/w862589/codyssey-precourse

$ cat hello.txt
Hello Linux

$ ls -la archive/
... .
... ..
# 일반 파일이 없는 빈 디렉터리임을 확인
```

`pwd`는 현재 위치, `ls -la`는 숨김 파일을 포함한 목록, `cp`는 복사, `mv`는 이름 변경/이동, `rm`은 삭제, `touch`는 빈 파일 생성, `cat`은 파일 내용 확인을 수행한다.

### 5.2 파일·디렉터리 권한 변경

```bash
# 실행 위치: ~/codyssey-precourse/01_terminal
ls -ld hello.txt archive
chmod 600 hello.txt
chmod 700 archive
ls -ld hello.txt archive
chmod 644 hello.txt
chmod 755 archive
ls -ld hello.txt archive
```

| 대상 | 변경 전 | 변경 후 | 복구 후 |
|---|---|---|---|
| `hello.txt` | `644` (`-rw-r--r--`) | `600` (`-rw-------`) | `644` |
| `archive/` | `755` (`drwxr-xr-x`) | `700` (`drwx------`) | `755` |

권한에서 `r=4`, `w=2`, `x=1`이다. `644`는 소유자 읽기·쓰기(6), 그룹 읽기(4), 기타 사용자 읽기(4)를 의미한다. 디렉터리의 `x`는 내부 경로에 접근할 수 있는 권한이다.

**증거:** [501_권한변경.jpg](evidence/501_권한변경.jpg) / [상세 로그](evidence/실행_검증_로그.md)

## 6. Docker 설치 및 기본 운영

**실행 위치:** Windows PowerShell(초기 설치 점검) 또는 WSL Ubuntu bash(Docker 운영). Docker 명령어는 Docker 엔진으로 전달되며, 이 단계에서는 컨테이너 내부로 진입하지 않는다.

### 6.1 Docker 버전 및 데몬 확인

```powershell
# Windows PowerShell
wsl --list --verbose
docker --version
docker info
```

실제 확인 결과:

```text
WSL: docker-desktop (Running, Version 2), Ubuntu (Running, Version 2)
Docker version 29.9.2, build a82e528
Docker Engine(Server) Version: 29.9.2
Context: desktop-linux
Containers: 0, Images: 0    # 최초 점검 당시
```

이는 Docker CLI와 엔진 간 통신이 가능하며 처음에는 Docker 이미지·컨테이너가 없는 상태였다는 뜻이다.

### 6.2 hello-world 실행 및 이미지/컨테이너 조회

```bash
# 실행 위치: WSL Ubuntu bash, 프로젝트 루트
docker run --name codyssey-hello hello-world
docker images
docker ps
docker ps -a
docker logs codyssey-hello
```

실제 확인 결과:

```text
Hello from Docker!
This message shows that your installation appears to be working correctly.

$ docker ps
CONTAINER ID   IMAGE   COMMAND   CREATED   STATUS   PORTS   NAMES

$ docker ps -a
... hello-world  "/hello"  ...  Exited (0) ... codyssey-hello
```

이미지(`hello-world:latest`)는 다운로드된 채 남지만, 해당 이미지를 실행한 컨테이너는 메시지 출력 후 종료된다. `docker ps`에는 실행 중인 컨테이너만, `docker ps -a`에는 종료된 컨테이너도 포함된다. `docker logs`로 종료된 컨테이너의 출력도 다시 확인했다.

**증거:** [502_컨테이너_실행.jpg](evidence/502_컨테이너_실행.jpg)

### 6.3 Ubuntu 컨테이너 내부 진입·재시작·exec

```bash
# WSL Ubuntu에서 실행 → 이후 컨테이너 내부 bash로 변경
docker run -it --name codyssey-ubuntu ubuntu:24.04 bash

# 아래는 Docker 컨테이너 내부 (root@...:/#)
pwd
whoami
cat /etc/os-release
ls -la
echo "Hello from Ubuntu Container" > /tmp/container-test.txt
cat /tmp/container-test.txt
exit

# 다시 WSL Ubuntu의 bash
docker ps -a
docker start -ai codyssey-ubuntu

# 다시 컨테이너 내부: 종료 전 파일 보존 확인
cat /tmp/container-test.txt
exit

# WSL Ubuntu: 기존 컨테이너를 실행한 뒤 별도 bash 추가
docker start codyssey-ubuntu
docker ps
docker exec -it codyssey-ubuntu bash

# 컨테이너 내부의 추가 bash만 종료
exit

# WSL Ubuntu: 컨테이너는 계속 Up인지 확인
docker ps
```

핵심 결과:

```text
root@e3461c3659f7:/# whoami
root
root@e3461c3659f7:/# cat /tmp/container-test.txt
Hello from Ubuntu Container

# 첫 번째 exit 뒤 재시작해도 같은 파일 확인
# docker exec로 연 추가 bash에서 exit 후에도 원래 컨테이너 Up
```

`docker start -ai`는 기존 컨테이너의 주 프로세스 입출력에 연결한다. 반면 `docker exec -it ... bash`는 실행 중인 컨테이너에 추가 bash를 실행한다. 종료된 컨테이너의 쓰기 계층은 **컨테이너 자체를 삭제하기 전까지** 보존될 수 있다.

### 6.4 리소스 및 중지 상태

```bash
# WSL Ubuntu
docker stats --no-stream codyssey-ubuntu
docker stop codyssey-ubuntu
docker ps -a
```

실제 측정:

```text
NAME              CPU %   MEM USAGE / LIMIT    MEM %    PIDS
codyssey-ubuntu   0.00%   1.883MiB / 31.3GiB   0.01%    1

# docker stop 실행 후
$ docker stop codyssey-ubuntu
codyssey-ubuntu
$ docker ps -a
... codyssey-ubuntu ... Exited (137) ...
```

`Exited (137)`은 SIGKILL에 의해 종료되었다는 의미로, 단순한 정상 종료 코드 0과 다르다. 정지 상태는 확인했지만 SIGKILL에 이르게 된 구체적인 경위까지 확정한 것은 아니다.

**상세 기록:** [Docker 운영 로그](evidence/실행_검증_로그.md)

## 7. Dockerfile 기반 커스텀 웹 서버

### 7.1 베이스 이미지와 커스텀 포인트

- **베이스 이미지:** `nginx:alpine` (정적 웹페이지를 제공할 NGINX 이미지)
- **커스텀 1:** `COPY`로 제작한 HTML을 NGINX의 기본 문서 디렉터리에 추가
- **커스텀 2:** `EXPOSE 80`으로 컨테이너 웹 서비스 포트를 선언

`02_webserver/Dockerfile`:

```dockerfile
FROM nginx:alpine
COPY site/ /usr/share/nginx/html/
EXPOSE 80
```

현재 `02_webserver/site/index.html`은 다음과 같이 **최초 HTML 상태로 복구**했다.

```html
<h1>Hello from Codyssey Docker!</h1>
```

바인드 마운트 실험에서 사용한 변경본은 [`evidence/bind-mount-after.html`](evidence/bind-mount-after.html)에 보존했다. 최초 Git Commit `b4faffb`에는 변경 후 문구가 있었으나, 이번 보완 작업에서 두 상태를 분리했다.

### 7.2 빌드 및 이미지 확인

```bash
# WSL Ubuntu, 프로젝트 루트
docker build -t codyssey-web:1.0 ./02_webserver
docker images
```

실제 확인 결과:

```text
[+] Building ... (7/7) FINISHED
... [2/2] COPY site/ /usr/share/nginx/html/
... naming to docker.io/library/codyssey-web:1.0

IMAGE                ID
codyssey-web:1.0     6ade5b6f782e
```

`docker build`는 Dockerfile의 지시를 바탕으로 커스텀 이미지를 만든다. `-t`는 이미지 이름과 태그를 지정한다.

## 8. 포트 매핑 및 브라우저 접속

```bash
# WSL Ubuntu: 같은 이미지에서 서로 다른 두 컨테이너 실행
docker run -d --name codyssey-web-8080 -p 8080:80 codyssey-web:1.0
docker run -d --name codyssey-web-8081 -p 8081:80 codyssey-web:1.0
docker ps
```

실제 확인 결과:

```text
codyssey-web-8080  Up ...  0.0.0.0:8080->80/tcp
codyssey-web-8081  Up ...  0.0.0.0:8081->80/tcp
```

Windows Chrome으로 `http://localhost:8080` 및 `http://localhost:8081`에 접속한 결과, 두 페이지 모두 다음 내용을 표시했다.

```text
Hello from Codyssey Docker!
```

호스트 포트 `8080`·`8081`이 각각 컨테이너 내부 NGINX의 `80`번 포트로 연결된다. 하나의 이미지로 별도 컨테이너 두 개를 실행할 수 있음을 확인했다. `EXPOSE 80`만으로는 호스트 포트가 게시되지 않으며 `-p`로 실제 매핑해야 한다.

**접속 증거(주소창 포함):** [8080](evidence/503_포트매핑_8080.png) / [8081](evidence/504_포트매핑_8081.png)

## 9. 바인드 마운트: 원본 변경이 컨테이너에 반영되는가?

### 9.1 원본 디렉터리를 연결하여 실행

```bash
# WSL Ubuntu, 프로젝트 루트
docker run -d --name codyssey-web-bind -p 8082:80 --mount type=bind,source="$(pwd)/02_webserver/site",target=/usr/share/nginx/html,readonly codyssey-web:1.0
```

- `source`: WSL Ubuntu의 실제 파일 디렉터리
- `target`: 컨테이너 내부 NGINX의 웹 문서 디렉터리
- `readonly`: 컨테이너에서 원본 파일을 수정하지 못하도록 하는 옵션. **호스트에서 원본을 수정한 결과는 읽을 수 있다.**

8082로 접속했을 때의 초기 출력:

```text
Hello from Codyssey Docker!
```

### 9.2 호스트 HTML 수정 전후 비교

```bash
# WSL Ubuntu, 프로젝트 루트
echo '<h1>Bind Mount Updated Successfully!</h1>' > 02_webserver/site/index.html
cat 02_webserver/site/index.html
```

변경 뒤 각 URL의 실제 관찰 결과:

| URL | 변경 후 표시된 내용 | 이유 |
|---|---|---|
| `http://localhost:8080` | `Hello from Codyssey Docker!` | 기존 이미지에 복사된 HTML 사용 |
| `http://localhost:8081` | `Hello from Codyssey Docker!` | 기존 이미지에 복사된 HTML 사용 |
| `http://localhost:8082` | `Bind Mount Updated Successfully!` | 원본 디렉터리를 실시간 참조 |

**증거:** [변경 전 8082](evidence/505_바인드마운트_변경전.png) / [변경 후 8082](evidence/506_바인드마운트_변경후.png)

### 9.3 보완 검증: 변경 전 → 변경 후 → 복구 (2026-10-24)

최초 Commit에 남아 있던 변경 후 HTML을 별도 파일에 복사한 뒤, `index.html`을 최초 문구로 복구했다. 바인드 마운트 중인 컨테이너는 재시작하지 않았다.

```bash
mkdir -p evidence
cp 02_webserver/site/index.html evidence/bind-mount-after.html
printf '<h1>Hello from Codyssey Docker!</h1>\n' > 02_webserver/site/index.html
curl -fsS http://localhost:8080
curl -fsS http://localhost:8082
# 둘 다 최초 문구

cp evidence/bind-mount-after.html 02_webserver/site/index.html
curl -fsS http://localhost:8080
curl -fsS http://localhost:8082
# 8080: Hello from Codyssey Docker!
# 8082: Bind Mount Updated Successfully!

printf '<h1>Hello from Codyssey Docker!</h1>\n' > 02_webserver/site/index.html
curl -fsS http://localhost:8082
# 복구됨: Hello from Codyssey Docker!
```

실행 결과는 [보완 재검증 로그](evidence/보완_재검증_로그.md)의 2~3절에 기록했다. 이 실험 뒤 **원본 `index.html`은 최초 문구를 유지**한다.

### 9.4 새 이미지 재빌드 및 새 컨테이너의 독립 실행

```bash
docker build -t codyssey-web:verify ./02_webserver
docker run -d --name codyssey-web-verify -p 8083:80 codyssey-web:verify
curl -fsS http://localhost:8083
# <h1>Hello from Codyssey Docker!</h1>
docker rm -f codyssey-web-verify
```

실제 빌드는 `(7/7) FINISHED`로 성공했고, `curl` 결과도 위와 같았다. 테스트 컨테이너 삭제 후 기존 웹 서버들은 그대로 실행 중이었다. 빌드 단계에 `CACHED`가 포함돼 있어, 완전한 무캐시 빌드 또는 다른 PC에서의 재현성까지 검증했다고 주장하지는 않는다. [검증 결과](evidence/보완_재검증_로그.md)

## 10. Docker 볼륨: 컨테이너 삭제 후 데이터 유지

```bash
# WSL Ubuntu
docker volume create codyssey-data
docker run -d --name codyssey-volume-a -v codyssey-data:/data ubuntu:24.04 sleep infinity
docker exec codyssey-volume-a sh -c 'echo "My data survives!" > /data/test.txt'
docker exec codyssey-volume-a cat /data/test.txt

# 첫 번째 컨테이너를 삭제한 뒤, 같은 볼륨으로 새 컨테이너 생성
docker rm -f codyssey-volume-a
docker run -d --name codyssey-volume-b -v codyssey-data:/data ubuntu:24.04 sleep infinity
docker exec codyssey-volume-b cat /data/test.txt
docker volume ls
```

실제 확인 결과:

```text
$ docker exec codyssey-volume-a cat /data/test.txt
My data survives!

$ docker rm -f codyssey-volume-a
codyssey-volume-a

$ docker exec codyssey-volume-b cat /data/test.txt
My data survives!

$ docker volume ls
DRIVER    VOLUME NAME
local     codyssey-data
```

즉, `codyssey-volume-a`를 삭제했는데도 독립적으로 관리되는 `codyssey-data` 볼륨을 연결한 새로운 컨테이너에서 동일한 파일을 읽을 수 있었다. 바인드 마운트는 지정한 호스트 경로와 연결하지만 Docker named volume은 Docker가 관리하는 저장 영역을 사용한다.

## 11. Git · GitHub · VS Code 연동

### 11.1 로컬 Git 저장소와 Commit

```bash
# WSL Ubuntu, 프로젝트 루트
git config --global init.defaultBranch main
git init -b main
git add 01_terminal 02_webserver
git status

# 실제 실습에서는 user.name과 user.email을 로컬 저장소에 설정함.
# 개인정보 노출을 피하기 위해 이메일 주소는 이 문서에 적지 않음.
git config --get user.name
git config --get user.email

git commit -m "Initial commit: Linux practice and Docker web server"
git status
git log -1 --oneline
```

실제 결과:

```text
[main (root-commit) b4faffb] Initial commit: Linux practice and Docker web server
4 files changed, 5 insertions(+)
On branch main
nothing to commit, working tree clean
b4faffb (HEAD -> main) Initial commit: Linux practice and Docker web server
```

**Git 설정 추가 검증 (2026-10-24):** `git config --list`를 실행하여 필요한 항목을 필터링하고 개인 이름·이메일 주소를 마스킹한 기록을 보존했다.

```bash
git config --list | grep -E '^(init.defaultbranch|user.name|user.email|remote.origin.url)=' | sed -E 's/^user\.(name|email)=.*/user.\1=[REDACTED]/' | tee evidence/git-config-sanitized.txt
```

```text
init.defaultbranch=main
user.name=[REDACTED]
user.email=[REDACTED]
remote.origin.url=https://github.com/PassionWoo/codyssey-precourse.git
```

`git status -sb` 결과는 `## main...origin/main`이었다. 이는 확인 당시 로컬과 원격의 추적 브랜치가 연결되어 있으며, 상태 출력에 앞서거나 뒤처진 커밋 수가 표시되지 않았다는 뜻이다. 현재 작업 파일 변경 사항은 새 Commit 전까지 원격 저장소에 반영되지 않는다. 제출된 로그는 **`git config --list`의 민감정보를 제외한 주요 항목 발췌본**이며 전체 원본 출력을 공개하지 않는다. [마스킹 설정 로그](evidence/git-config-sanitized.txt)

### 11.2 VS Code와 WSL 연결

처음에는 VS Code가 Windows PowerShell로 Ubuntu 저장소를 UNC 경로(`\\wsl.localhost\...`)에서 열어, Windows Git이 소유권 오류를 반환했다. 이후 WSL 확장을 설치하고 아래 명령으로 Ubuntu 원격 모드에 연결했다.

```bash
# WSL Ubuntu bash에서 실제 실행한 원격 연결 명령
code --remote wsl+Ubuntu /home/w862589/codyssey-precourse
```

다음 두 명령을 VS Code의 **WSL Ubuntu 터미널**에서 실행하여 Git 연동을 확인했다.

```bash
pwd
git status
```

```text
/home/w862589/codyssey-precourse
On branch main
nothing to commit, working tree clean
```

**증거:** [VS Code WSL·Git 화면](evidence/507_VSCode_WSL_Git연결.png)

### 11.3 GitHub 연결 및 게시

VS Code Source Control의 **Publish Branch** 기능으로 GitHub 계정을 인증하고 `PassionWoo/codyssey-precourse` 원격 저장소를 게시했다. 웹 화면에서 `main` 브랜치, 최초 Commit `b4faffb`, 두 실습 디렉터리 및 **Private** 상태를 확인했다.

- 저장소: [https://github.com/PassionWoo/codyssey-precourse](https://github.com/PassionWoo/codyssey-precourse)
- 최초 게시 상태: **Private** (평가자가 접근할 수 있도록 제출 규칙에 맞게 조정 필요)
- GitHub에는 **소스코드와 Git 이력**이 게시되며, 실행 중인 Docker 컨테이너나 로컬 볼륨의 실제 데이터가 자동으로 업로드되지는 않는다.

**증거:** [GitHub 저장소 화면](evidence/508_GitHub_저장소.png)

## 12. 트러블슈팅 (실제 발생 사례)

### 사례 1. Ubuntu 터미널에 붙여넣기 제어문자가 명령으로 입력됨

| 구분 | 내용 |
|---|---|
| 문제 | WSL Ubuntu 터미널에서 붙여넣기를 시도했을 때 `^V` 및 `command not found` 발생 |
| 원인 가설 | 터미널의 붙여넣기 단축키가 아닌 제어문자 입력으로 처리됨 |
| 확인 | 오류 뒤에 입력한 `pwd`, `whoami`, `ls -la`는 정상 실행됨 |
| 해결/대안 | 제어문자를 명령으로 사용하지 않고 실제 명령만 입력하여 작업 지속. 터미널별 붙여넣기 단축키·우클릭 방식을 사용 |
| 결과 | 이후 Linux 파일 조작 실습 정상 수행 |

### 사례 2. VS Code에서 Windows Git이 WSL 저장소를 신뢰하지 못함

| 구분 | 내용 |
|---|---|
| 문제 | VS Code 터미널의 `git status` 실행 시 `fatal: detected dubious ownership` 발생 |
| 원인 가설 | VS Code가 WSL 원격 세션이 아닌 **Windows PowerShell**을 사용하여 Ubuntu 파일을 `\\wsl.localhost\...` 네트워크 경로로 열었기 때문 |
| 확인 | 프롬프트가 `PS Microsoft.PowerShell.Core\FileSystem::\\wsl.localhost\...>`로 표시됨. 기존 WSL bash에서는 같은 저장소의 `git status`가 정상 수행됨 |
| 해결 | `safe.directory` 예외를 추가하지 않고 VS Code를 **WSL Ubuntu 원격 모드**로 다시 연결 |
| 결과 | VS Code WSL 터미널에서 `git status` → `On branch main`, `working tree clean` 확인 |

### 사례 3. VS Code의 WSL 연결 명령을 검색할 수 없음

| 구분 | 내용 |
|---|---|
| 문제 | 명령 팔레트에서 `WSL: Reopen Folder in WSL`, `WSL: Connect to WSL`을 검색해도 일치하는 명령이 나오지 않음 |
| 원인 가설 | 처음에는 WSL 확장이 없었고, 설치 직후에도 현재 창에서 예상 명령을 사용할 수 없었음. 정확한 내부 원인은 확인하지 못함 |
| 확인 | WSL 확장 설치 화면을 확인했으나 명령 검색 결과는 계속 `No matching results`로 표시됨 |
| 해결/대안 | 이미 실행 중인 WSL Ubuntu의 bash에서 `code --remote wsl+Ubuntu /home/w862589/codyssey-precourse`를 사용해 원격 창 실행 |
| 결과 | VS Code에서 `[WSL: Ubuntu]` 표시 및 정상 Git 상태 확인 |

**보충:** `docker stop codyssey-ubuntu` 뒤에 `Exited (137)`을 관찰했지만, 종료 신호의 상세 원인까지 확인하지 않았으므로 해결된 트러블슈팅 건수에 포함하지 않았다.

## 13. 다른 환경에서 재현하는 방법

필요 도구: Docker Engine 또는 Docker Desktop, Docker CLI, Git. Linux 셸(bash)을 사용하는 예제이며 Windows에서는 WSL 2 Ubuntu를 권장한다.

```bash
# 1) 프로젝트 받기 (접근 권한 필요)
git clone https://github.com/PassionWoo/codyssey-precourse.git
cd codyssey-precourse

# 2) 최초 실습 HTML 확인 (GitHub에 보완본 반영 후)
cat 02_webserver/site/index.html
# <h1>Hello from Codyssey Docker!</h1>

# 3) 이미지 빌드
docker build -t codyssey-web:1.0 ./02_webserver

# 4) 호스트 포트가 비어 있을 때 컨테이너 2개 실행
docker run -d --name verify-web-8080 -p 8080:80 codyssey-web:1.0
docker run -d --name verify-web-8081 -p 8081:80 codyssey-web:1.0

# 5) 응답 및 포트 확인
curl http://localhost:8080
curl http://localhost:8081
docker ps

# 6) 바인드 마운트: 변경 반영 실험
docker run -d --name verify-web-bind -p 8082:80 --mount type=bind,source="$(pwd)/02_webserver/site",target=/usr/share/nginx/html,readonly codyssey-web:1.0
curl http://localhost:8082
printf '<h1>Bind Mount Updated Successfully!</h1>\n' > 02_webserver/site/index.html
curl http://localhost:8080
curl http://localhost:8081
curl http://localhost:8082
```

**예상:** 처음에는 세 포트에서 `Hello from Codyssey Docker!`를 반환하고, 소스 변경 후에는 8082에서만 `Bind Mount Updated Successfully!`를 반환한다. 명령은 Docker가 설치되어 있고 해당 포트가 비어 있는 환경을 전제로 한다. `curl` 사용이 어려우면 브라우저 주소창에서 각각 접속하여 확인할 수 있다.

볼륨 검증은 다음과 같이 독립적으로 재현할 수 있다.

```bash
docker volume create verify-codyssey-data
docker run -d --name verify-vol-a -v verify-codyssey-data:/data ubuntu:24.04 sleep infinity
docker exec verify-vol-a sh -c 'echo "My data survives!" > /data/test.txt'
docker rm -f verify-vol-a
docker run -d --name verify-vol-b -v verify-codyssey-data:/data ubuntu:24.04 sleep infinity
docker exec verify-vol-b cat /data/test.txt
# 예상: My data survives!
```

> 이미 동일한 이름의 컨테이너가 있거나 8080~8082 포트가 사용 중이면 실행 전에 기존 컨테이너를 정리하거나 호스트 포트를 변경해야 한다. 다른 컴퓨터에서는 `/home/w862589/...` 같은 사용자 고유 경로를 그대로 사용하지 않고 **프로젝트 디렉터리에서 `$(pwd)`를 사용**한다. 재현 예제의 파일 수정은 Git 작업 트리에 반영되므로 실험 후 `git status`로 확인하고, 필요하면 `git restore 02_webserver/site/index.html`로 원상복구할 수 있다. `git restore`는 아직 Commit하지 않은 수정 내용을 덮어쓰므로 **검증용 복제 저장소에서만** 사용한다.

## 14. 보안 및 제출 전 확인

- 이 문서는 비밀번호·토큰·개인키·인증 코드의 값을 포함하지 않는다. 공유하는 터미널 원본 로그도 이러한 내용을 먼저 확인하고 마스킹한다.
- Git Commit의 작성자 이메일은 Git 이력에서 확인될 수 있다. 저장소를 공개하기 전에 개인정보 공개 여부를 확인하고, 필요하면 GitHub의 `noreply` 이메일을 사용한다.
- 프로젝트 저장소는 현재 **Private**이다. Codyssey 평가자가 접근하도록 공개 범위를 조정하거나 지정된 계정을 초대해야 한다.
- 증거 파일은 README의 상대경로 링크(`evidence/...`)가 실제로 열리는지 GitHub에서 확인한다.
- 최종 업데이트에는 `README.md`와 `evidence/`를 `git add` → `git commit` → `git push`하여 반영한다.

## 15. 최종 학습 결과

이번 실습을 통해 **CLI로 작업 디렉터리와 권한을 관리하고, Docker로 재현 가능한 웹 서버 실행 환경을 구성하며, Git/GitHub에 그 소스와 작업 이력을 남기는 일련의 흐름**을 수행했다.

특히 이미지와 컨테이너의 차이, 실행 중인 컨테이너와 종료된 컨테이너의 차이, 포트 매핑을 통한 호스트↔컨테이너 연결, 바인드 마운트의 변경 반영, 볼륨의 데이터 영속성을 모두 직접 실행하여 검증했다. VS Code도 단순 Windows 폴더 열기와 WSL 원격 접속의 차이를 경험한 뒤 올바른 작업 환경에 연결했다.

---

**제출 대상:** GitHub Repository 링크 — [PassionWoo/codyssey-precourse](https://github.com/PassionWoo/codyssey-precourse)
**검증 근거:** 이 README의 명령·출력, [기존 실행 로그](evidence/실행_검증_로그.md), [보완 재검증 로그](evidence/보완_재검증_로그.md), [evidence/ 증거 이미지](evidence/)
