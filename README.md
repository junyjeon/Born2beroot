# Born2beroot

- 목차

# 📃요구사항📃

1. `aptitude`와 `apt`의 차이점, `SELinux`와 `AppArmor`가 무엇인지 등을 설명할 수 있어야 한다.
2. `SSH` 서비스는 4242 포트만 사용해야하며 이러한 이유로 `SSH`를 `root`로 연결할 수 없어야 한다
3. `UFW` 방화벽을 이용해야하기 때문에 4242 포트만 열어놔야 한다.
4. Virtual machine을 실행할 때 방화벽이 활성화 되어 있어야 한다.
5. Virtual machine의 hostname은 42로 끝나야 한다.
6. 강력한 암호 정책을 구현해야 한다. (sudo 설치 및 사용)
7. 강력한 비밀번호 정책
    - 비밀번호는 30일 마다 만료되어야 한다.
        
        `PASS_MAX_DAYS 30`
        
    - 비밀번호 수정 전까지 허용되는 최소 일수는 2로 설정해야 한다.
        
        `PASS_MIN_DATS 2`
        
    - 사용자는 비밀번호 만료 7일 전에 경고 메시지를 받아야 한다.
        
        `PASS_WARN_AGE 7`
        
    - 비밀번호는 10자 이상이어야 하고
        
        `minlen=10`
        
        대문자와 숫자를 포함해야 한다.
        
        `ucredit=-1 dcredit=-1`
        
        소문자와 특수 문자는 credit을 올리지 않음
        
        `lcredit=0 ocredit=0`
        
        또한 3개 이상의 연속된 동일한 문자를 포함해서는 안된다.
        
        `maxrepeat=3`
        
    - 비밀번호에 사용자 이름이 포함 되어서는 안된다.
        
        `reject_username`
        
    - 이전 비밀번호와 최소 7자 이상 달라야 한다.
        
        difok=7
        
    - 물론 root 비밀번호는 이 정책을 준수해야 한다.
    - configuration file 세팅 후 root 계정 포함 가상 머신의 모든 계정의 비밀번호를 변경해야 한다.
    
    `/etc/login.defs`
    PASS_MAX_DAYS 30 비밀번호는 30일마다 만료
    PASS_MIN_DATS 2 비밀번호는 2일부터 수정 가능
    PASS_WARN_AGE 7 만료되기 7일 전에 경고 메시지
    
    `/etc/pam.d/common-password`
    
    minlen=10 10자 이상, ucredit=-1 lcredit=-1 dcredit=-1 대소문자와 숫자 포함 되어야 함, maxrepeat=3 같은 문자는 3개 까지만,  reject_username 사용자 이름 포함 되면 안됨, enforce_for_root 다음에 오는 규칙은 root 빼고 적용, difok=7 이전 암호의 일부가 아닌 7자 이상이어야 함
    
    Virtual machine에 있는 모든 계정 암호 정책에 맞게 변경해야 함 `chage`
    
8. root user 외에도 username으로 로그인한 user가 있어야 한다.(이 사용자는 user42와 sudo groups에 속해있어야 함)
9. Defense 시에 새로운 사용자를 생성하고 그룹에 등록해야 한다.
    
    `$sudo adduser [사용자명]` 사용자 생성
    
    `$sudo groupadd [그룹명]` 그룹 생성
    
    `$groups` 그룹 확인
    
    `$usermod -aG [그룹명] [사용자명]` 사용자를 그룹에 추가
    
10. sudo
    
    visudo로 /etc/sudoers.tmp로 이동
    
    ```jsx
    Defaults	log_input
    Defaults	log_output
    ```
    
    ```jsx
    Defaults	iolog_dir="/var/log/sudo/”
    ```
    
    ```jsx
    Defaults	passwd_tries=3 
    ```
    
    ```jsx
    Defaults	authfail_message=”authentication fail” 
    ```
    
    ```jsx
    Defaults badpass=message=”password fail” 
    ```
    
    ```jsx
    Defaults	requirettyTTY
    ```
    
    ```jsx
    Defaults secure_path=”/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin”
    ```
    
11. [monitoring.sh](http://monitoring.sh) Shell 스크립트
    
    <aside>
    💡 서버 시작 시 스크립트는 10분마다 모든 터미널에 대한 아래 정보들을 표시한다. (배너는 옵션, 에러가 보이면 안됨)
    
    </aside>
    
    OS의 architecture 및 kernel version
    
    physical(물리적) processors 개수
    
    virtual(가상) processors 개수
    
    서버에서 사용 가능한 RAM 및 사용량(%)
    
    서버에서 사용 가능한 메모리 및 사용량(%)
    
    processors 사용량(%)
    
    마지막 부팅 날짜 및 시간
    
    LVM 활성화 여부
    
    작동 중인 연결 개수
    
    서버를 사용하고 있는 이용자 수
    
    서버의 IPv4와 MAC주소
    
    sudo 프로그램으로 실행된 명령의 수
    
12. LVM이란? LVM를 이용해 파티션 생성
13. 평가 중에 이 호스트 이름을 수정해야 합니다.
    
    groupadd user42
    usermod -aG sudo,user42 <사용자명>
    usermod -g user42 <사용자명>
    
14. apt-get install net-tools
    
    ifconfig
    
15. DHCP 끄기

# 🧾평가준비🧾

1. 가상 머신의 작동방식
    
    > 가상 머신은 pc 나 원격 서버에서 cpu, 메모리, 스토리지를 빌려와 작동됩니다.
    > 
    
    가상 머신은 물리적 호스트 컴퓨터 또는 클라우드 서버와 같은 원격 서버에서 빌려 온
     CPU,  메모리, 스토리지를 사용하여 실제 컴퓨터처럼 동작하는 컴퓨터 파일을 만드는 프로세스 입니다. 창에서 실행될 수도 있고 가상머신 내의 소프트웨어가 주 운영체제를 방해할 수 없습니다.
    
2. `CentOS`
    
    패치가 빠르며 유틸의 양이 많고 관리 툴 성능이 우수하고 호환성이 좋다.
    
    서버용 리눅스는 대부분 CentOS가 쓰임
    
    `Debian`
    
    편한 설치와 이용법 덕분에 진입장벽이 낮다.
    
    개인용 리눅스는 Ubuntu
    
3. 가상 머신의 목적
    
    > 단일 컴퓨터에서 다른 여러 운영체제를 동시에 실행할 수 있게 하기위해
    > 
4. APT
    
    > 소프트웨어 설치와 제거, 업데이트를 처리하는 패키지 관리 툴
    > 
    
    `/etc/apt/sources.list`
    
    Aptitude
    
    > APT보다 소스 목록이 방대하고 apt-get, apt-cache을 포함한다. 설치된 패키지 목록 표시, 업그레이드에 사용할 수 없는 패키지 보관, 사용할 수 없는 패키지 삭제, 충돌이 일어나면 해결방법 제시
    > 
5. UFW 서비스 시작
    
    `$sudo systemctl status ufw`
    
6. SSH 서비스 시작
    
    `$sudo systemctl status ssh` 
    
7. 그룹 생성 및 확인
    
    `$sudo adduser [사용자명]` 사용자 생성
    
    `$sudo groupadd [그룹명]` 그룹 생성
    
    `$groups` 그룹 확인
    
    `$usermod -aG [그룹명] [사용자명]` 사용자를 그룹에 추가
    
8. 이 암호 정책의 장단점 설명
    
    > 무차별 대입 공격 방어 등 사용자의 암호 보안을 강화 할 수 있다.
    > 
9. 호스트이름 확인
    
    `$sudo hostnamectl`
    
10. 호스트 아이디 변경 후 재시작
    
    `$sudo hostnamectl set-hostname —static  [호스트명]`
    
    exit으로 나간 후 다시 로그인
    
11. LVM 개념
    
    > 물리적 공간을 합친 뒤 사용자가 필요에 따라 논리적 공간으로 나눠 효율적으로 사용 할 수 있게 만든다. (논리 공간 관리자)
    > 
12. sudo 설치 확인
    
    `$sudo dpkg -l sudo` dpkg -l  설치된 패키지 조회
    
    `$sudo gpasswd -a [사용자명] [그룹명]` 새 사용자를(7. 사용자 생성) 그룹에 추가
    
13. sudo정책 확인
    
    `$sudo visudo`
    
14. sudo 로그 확인
    
    `$sudo ls /var/log/sudo`
    
15. sudo가 사용되는 이유
    
    > 관리자 권한으로 실행하기 위해
    > 
16. UFW확인
    
    `$sudo systemctl status ufw`
    
17. UFW 개념
    
    > 
    > 
18. UFW 활성 규칙
    
    `$sudo ufw status`
    
19. 8080포트 열기
    
    `$sudo ufw allow 8080`
    
    `$sudo ufw status verbose`verbose는 장황하게 라는 뜻으로 조금 더 자세히 나온다.
    
20. 8080포트 닫기
    
    `$sudo ufw delete allow8080`
    
21. SSH 개념
    
    > 
    > 
22. SSH가 4242포트만 사용하는지 확인
    
    `$ss -tunlp`
    
    -t : tcp 유형만 표시
    -u : udp 유형만 표시
    -n : 호스트와 포트, 사용자 이름 숫자로 표시
    -l : listen 상태 소켓 확인
    -p : 소켓 사용하는 프로세스 정보 표시
    
23. 새로 만든 유저로 SSH 연결.
    
    `$ssh [사용자명]@[ip주소] -p 4242`
    
    `vi /etc/ssh/sshd_config`
    
24. `cron`이 무엇? 유닉스 계열 스케쥴러
    
    `service cron stop`
    
    `service cron restart`
    
    `service cron status`
    
25. hostnamectl
    
    `sudo hostnamectl set-hostname <변경할 hostname>`
    

# 개념

# VirtualBox

`InnoTek`이 개발하고 현재 `Oracle`에서 배포되고 있는 VM이다

버추얼박스는 원격으로 가상 컴퓨터를 제어할 수 있음

<aside>
💡 VM(virtualedj klashjdkahfjkdasfhjklasdfhjklasdhfljkasdhfkasdhjkglhasdjklghasdjklfhasdjklfhklh

컴퓨터 시스템을 에뮬레이션 하는 소프트웨어를 일컫는 말로, 가상머신 상에서 운영체제나 응용 프로그램을 설치 및 실행할 수 있다.

</aside>

<aside>
💡 Kernel(알맹이)

OS는 Kernel과 system program으로 구분

컴퓨터의 자원을 관리하는 역할

사용자 -> 시스템 프로그램(Shell 등을 사용) -> 커널 -> 컴퓨터 자원 접근

```
물리적 자원 이름 -> 추상화한 자원 용어
CPU -> 태스크(Task)
메모리(memory) -> 페이지(page), 세그먼트(segment)
디스크(disk) -> 파일(file)
네트워크(network) -> 소켓(socket)
디바이스 드라이버(driver) pirnter, GPU
```

</aside>

apt-get update

apt-get install sudo

- visudo
    
    ```xml
    Defaults	authfail_message="권한 획득 실패 메세지"
    Defaults	badpass_message="비밀번호 틀릴때 메세지"
    Defaults	iolog_dir="/var/log/sudo/"
    Defaults	log_input
    Defaults	log_output
    Defaults	requiretty
    Defaults	passwd_tries=3
    ```
    

mkdir /var/log/sudo

- UFW
    
    https://webdir.tistory.com/m/206
    
    ufw enable 활성화
    
    ufw status verbose 상태
    
    ufw default deny 기본정책 거부
    
    들어오는 패킷에 대해서는 전부 거부(deny)
    
    나가는 패킷에 대해서는 전부 허가(allow)
    
    ufw allow 4242 SSH 4242번 포트 허용
    
    ufw status verbose
    
- SSH
    
    SSH 서비스는 포트 4242에서만 실행됩니다. 보안상의 이유로 SSH를 루트로 사용하여 연결할 수 없어야 합니다.
    
    sshd_config 에서 포트 4242, PermitRootLogin no
    
    systemctl restart ssh
    
    systemctl status ssh
    
    ssh junyojeo@192.168.56.1 -p 4242
    
    ssh -p 4242 포트 4242에 접근
    
- 패스워드 정책
    
    vi /etc/login.defs
    PASS_MAX_DAYS 30
    PASS_MIN_DATS 2
    PASS_WARN_AGE 7
    

apt-get -y install libpam-pwquality

vi /etc/pam.d/common-password
minlen=10 ucredit=-1 lcredit=-1 dcredit=-1 maxrepeat=3 reject_username enforce_for_root difok=7

passwd -e <사용자명>

# Debian

데비안 프로젝트라는 커뮤니티에서 개발중인 리눅스 배포판.(우분투 리눅스 파워셀 같은 종류)

유지보수가 쉽고 다른 배포판들에 비해 안정성을 우선시해서 보수적으로 채용된다.

무료

# CentOS(**C**ommunity **Ent**erprise **O**perating **S**ystem)

[`RHEL(Red Hat Enterprise Linux)`](https://namu.wiki/w/%EB%A0%88%EB%93%9C%ED%96%87%20%EC%97%94%ED%84%B0%ED%94%84%EB%9D%BC%EC%9D%B4%EC%A6%88%20%EB%A6%AC%EB%88%85%EC%8A%A4)에서 파생된 리눅스의 배포판.

서버용으로 네이버 카카오에서도 사용함.

무료

# KDump(The kexec-based Crash Dumping Solution)

kexec를 바탕으로 한 “커널 크래쉬 덤핑 메커니즘”이다. 이는 커널패닉(커널이 오류를 수신했을 때)이 발생하였을 때, BIOS를 거치지 않고(kexec의 역할) 시스템 메모리 상태를 vmcore 파일 형태로 생성하는 작업이다.

# AppArmor(Application Armor)와 SELinux(Security-Enhanced Linux)

AppArmor(Application Armor)

관리자가 프로필 별로 엑세스 권한을 제어할 수 있게 해주는 리눅스 커널 보안 모듈…

SUSE Linux, Debian, Ubuntu 운영체제에서 주로 사용되며, MAC(Mandatory Access Control 강제적 접근 통제)이 적용됨.

SELinux(Security-Enhanced Linux)

관리자가 시스템 엑세스 권한을 효과적으로 제어할 수 있게 해주는 리눅스시스템용 아키텍처..

미국 국가보안국이 LSM(Linux Security Module)리눅스 보안 모듈을 사용하여 개발함.

RHEL, Fedora, CentOS에서 많이 사용되며, MAC(Mandatory Access Control 강제적 접근 통제)이 적용됨.

# LVM(Logical Volume Manager)

논리적인 공간 관리자. 

LVM은 파티션(디스크 공간을 나눈 것) 대신에 volume이라는 단위로 저장 장치를 논리적으로 다룬다.

LVM은 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c9c8824b-0d25-468f-8c47-6066a3353da9/Untitled.png)

# SSH(**S**ecure **Sh**ell)

원격지에서 호스트 컴퓨터에 접속하기 위해 사용되는 인터넷 프로토콜이다. 1995년에 나온 프로토콜이며, 기본 포트는 22번이다. 이름대로 Shell로 원격 접속을 하는것이기 때문에 접속 후에도 CLI(Command line interface)에서 작업을 하게 된다. 기존에 사용되던 Telnet, FTP등은 암호화가 이루어지지 않아 계정정보 탈취위험이 높았지만, SSH는 모든 데이터의 암호화가 보장된다.

Key를 이용하여 보안을 구성하는데, 기본적으로 SSH key는 public key, private key 두가지로 이루어진다. 비공개 키는 로컬 머신(게스트)에 위치해야하며, 공개키는 리모트 머신(호스트)에 위치해야한다. SSH 접속을 시도하면 로컬 머신의 비공개키와 리모트 머신의 비공개키를 비교하여 일치하는지 확인하게된다.

```elixir
$ apt search openssh-sever# ssh가 설치되어있는지 검색
$ systemctl status ssh# ssh status 확인
$ apt install openssh-server# 설치
$ apt sudo ufw allow 4242# 4242Port를 개방
$ sudo systemctl restart ssh# ssh 재시작
```

### Guest SSH 설정파일 : sshd_config

```
$ sudo nano /etc/ssh/sshd_config
# ssh설정 파일, Port 4242로 바꿔준다.
```

- `PermitRootLogin no` 로 파일을 수정하여 Root 계정으로 ssh통신을 못하도록 할 수 있다. (보안상의 이유)
- **게스트(Virtualbox의 가상머신은) ip의 기본값은 `10.0.2.15`이다.**
- **호스트(iMac) ip는 $ipconfig 로 확인할 수 있다.**
- Virual box에서 데비안 - 설정 - 네트워크 어댑터로 가서 포트포워딩을 설정해주고 밑의 명령어를 통해 SSH연결을 할 수 있다. -> 참고 글 : https://mrgamza.tistory.com/506

# UFW**(Uncomplicated FireWall)**

UFW는 데비안 계열 리눅스 환경에서 작동하는 이름대로 "복잡하지 않은" 방화벽 관리 프로그램이다. 간단히 메뉴얼을 봤는데 **서비스명(ex: SSH), IP 주소, 포트 번호, Ping 요청** 등을 허용/거부 할 수 있는 기능을 제공한다.

본 과제에서는 기본 SSH 포트인 22번 포트를 닫고, 4242번 포트를 개방하는데 사용되었다.

```
$ sudo apt install ufw# 설치
$ sudo ufw status verbose# 작동 상태 확인
$ sudo ufw enable# 부팅시 ufw 활성화
$ sudo ufw allow 4242# 4242 Port 개방
$ sudo ufw default deny# 기본 정책을 차단
$ sudo ufw status numbered# 정책들에 번호를 붙여 나열하여 확인
$ sudo ufw delete <규칙번호># 정책번호로 삭제
```

# PAM(**Privileged Access Management)**

권한 접근 관리자.

PAM(Privileged Access Management)은 ID 보안 솔루션으로 중요한 리소스에 대한 무단 액세스를 모니터링, 검색 및 방지하여 사이버 위협에서 조직을 보호하는 데 도움이 됩니다. PAM은 사용자, 프로세스 및 기술의 조합을 통해 작동하며 권한 있는 계정을 누가 사용하고 있는지와 이들이 로그인 중에 무엇을 하는지에 대한 가시성을 제공합니다. 관리 기능에 액세스하는 사용자의 수를 제한하면 시스템 보안이 강화되고 추가된 보호 레이어가 위협 행위자의 데이터 위반을 완화합니다.

# 비밀번호 정책

- `$ vi /etc/login.defs`

```bash
PASS_MAX_DAYS 30: 패스워드 사용 가능 기간
PASS_MIN_DAYS 2: 패스워드 변경 후 최소 사용 기간
PASS_MIN_LEN  10: 패스워드 최소 길이
PASS_WARN_AGE 7: 패스워드 기간 만료 경고 기간
```

- `$ sudo apt install libpam-cracklib`  정책 설정 패키지 설치

```bash
$ sudo apt install libpam-cracklib
or $ sudo apt install libpam-pwquality
패스워드 유효성 제한 패키지
```

```bash
$ vi /etc/pam.d/common-password
```

```bash
retry=3 암호 입력 횟수 3회로 제한
minlen=10 암호 최소 길이 10 -위랑 중복
difok=7 기존 패스워드와 달라야하는 문자 수 7
ucredit=-1 대문자 1개 이상 포함 (-1 => 1개 이상)
lcredit=-1 소문자 1개 이상 포함
dcredit=-1 숫자 1개 이상 포함
maxrepeat=3 같은 문자가 3번 이상 연속으로 나오면 안됨
reject_username 사용자 이름이 암호에 그대로 들어있거나 뒤집혀 들어있지 않아야함
enforce_for_root 루트 사용자가 패스워드 변경을 할 때에도 위 내용을 적용
```

- `$ apt-get -y install libpam-pwquality`   모듈

```bash
passwd -e [계정]
```

## 모니터링

- 서버 시작 시 스크립트는 10분마다 모든 터미널에 대한 아래 정보들을 표시한다. (배너는 옵션, 에러가 보이면 안됨)
    
    `$ apt-get -y install sysstat`
    
    - OS의 architecture 및 kernel version
        
        `printf “#Architecture: “`
        
        `uname -a` 시스템에 대한 정보 출력
        
    - physical(물리적) processors 개수
        
        `printf "#CPU physical : "`
        
        `nproc —all` 현재 프로세스에서 사용 가능한 처리 장치의 수
        
    - virtual(가상) processors 개수
        
        `printf "#vCPU : "`
        
        `cat /proc/cpuinfo | grep processor`
        
    - 서버에서 사용 가능한 RAM 및 사용률(%)
        
        `printf "#Memory Usage: "`
        
        `free -h | grep Mem | awk ‘{printf “%d/%dMB (%.2f%%)\n”, $3, $2, $3/$2 * 100)’`free명령어가 메모리 사용량을 보여줌. `%d/%dMB (%.2f%%)`로 보임
        
    - 서버에서 사용 가능한 메모리 및 사용률(%)
        
        `printf “#Disk Usage: ”`
        
        `df -a -BM | grep /dev/map | awk '{sum+=$3}END{print sum}' | tr -d '\n' printf "/" df -a -BM | grep /dev/map | awk '{sum+=$4}END{print sum}' | tr -d '\n' printf "MB (" df -a -BM | grep /dev/map | awk '{sum1+=$3 ; sum2+=$4 }END{printf "%d", sum1 / sum2 * 100}' | tr -d '\n' printf "%%)\n”` df는 시스템 전체의 디스크 사용량(마운트된)
        
    - processors 사용률(%)
        
        `printf “#CPU load: ”`
        
        `mpstat | grep all | awk ‘{print “%.2f%%\n”, 100-$13}’` 
        
    - 마지막 재부팅 날짜 및 시간
        
        `printf "#Last boot: "` 
        
        `who -b`마지막 시스템 부팅 시간 출력
        
    - LVM 활성화 여부
        
        `printf "#LVM use: "`
        
        `if [ "$(lsblk | grep lvm | wc -l)" -gt 0 ] ; then printf "yes\n" ; else printf "no\n" ; fi`
        
        $(lsblk | grep lvm | wc -l)가 참이면 yes 아니면 no printf로 출력
        
    - 작동중인 연결 개수
        
        `printf "#Connections TCP : "`
        
        `ss | grep -i tcp | wc -l | tr -d '\n'
        printf " ESTABLISHED\n"`
        
    - 서버를 사용하고 있는 이용자 수
        
        `printf “#User log: “`
        
        `who | wc -l`
        
    - 서버의 IPv4와 MAC주소
        
        `printf "#Network: IP "`
        
        `hostname -I | tr -d '\n'
        printf "("
        ip link show | awk '$1 == "link/ether" {print $2}' | sed '2, $d' | tr -d '\n'
        printf ")\n"`
        
    - sudo 프로그램으로 실행된 명령의 수
        
        `printf “#sudo :”`
        
        `journalctl _COMM=sudo | wc -l | tr -d '\n'
        printf " cmd\n"`
        

wooshin

[https://www.notion.so/cocomhwa/Born2beroot-5dcb794819304e64a44735534821be00](https://www.notion.so/Born2beroot-5dcb794819304e64a44735534821be00?pvs=21)

hyeslim

[https://spiral-quince-c7d.notion.site/Born2beRoot-0ab5086773d5487ca435e72cab34d642](https://www.notion.so/Born2beRoot-0ab5086773d5487ca435e72cab34d642?pvs=21)

minseok

hyeonjun

하이퍼바이저 https://www.vmware.com/kr/topics/glossary/content/hypervisor.html

apptitude https://www.fosslinux.com/43884/apt-vs-aptitude.htm

apparmor chmod https://ko.wikipedia.org/wiki/AppArmor

ufw 방화벽 관리 프로그램 https://ko.wikipedia.org/wiki/UFW

ss -tunlp udp  삭제

ssh 원격 호스트에 접속하기 위해 사용되는 보안 프로토콜?

ssh 연결
