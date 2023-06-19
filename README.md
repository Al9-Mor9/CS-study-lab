# CS-study-lab
CS 스터디 실습을 위한 레포지토리입니다.   
실습에서는 C를 주언어로, Linux(Ubuntu) 환경에서 이루어집니다. 

<details><summary> <h3>Windows에서 Ubuntu 쓰기(WSL2)</h3> </summary>

```markdown
- WSL2을 이용한 리눅스 설치
윈도우에서 리눅스 서버를 설치하기 위해선 다음 4가지 방법이 있다.

1. PC의 기본 OS로 우분투 설치 - 윈도우 사용 불가.
2. WSL2 - 빠른 속도와 호환성
3. 가상머신 - 느리고 용량의 제한
4. 클라우드 - 안정적이고 가볍지만 유료

WSL(Windows Subsystem for Linux)을 통해 리눅스를 설치해 보자.

- WSL2가 WSL1과 다른 두 가지 큰 특징

    1. 빠른 I/O performance(increase file system performance)

       (평균 WSL1보다 3~6배정도 빠르고, 압축 풀기(unzipping a tarball) 같은 경우 약 20배까지 차이난다고 함)

    2. 100% system call compatibility(support full system call compatibility)

       (docker, VS code 등등...)
```

1. Windows Store에서 Windows Terminal 설치 
    
    windows powershell이나 터미널(앱)으로 실행가능 
    
2. 설치한 cmd에서 다음 2개 명령어를 사용해 사전 세팅 (관리자 권한)

```markdown
1. Linux용 Windows 하위시스템 옵션 기능 사용
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

2. Virtual Machine플랫폼 우선 기능을 사용
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
1. Windows Store에서 우분투 설치 (Unbuntu 20.04.6 LTS)
2. 개발자 모드 활성화, Windows 기능설정후 재부팅 
    linux용 windows 하위 시스템 체크
     .NET Framework 3.5 체크.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c0f2daa5-ffda-4d97-b140-db39d4389a48/Untitled.png)

1. 설치한 리눅스 버젼을 wsl2로 변경 (cmd)

```markdown
1. 버젼확인
wsl --list --verbose

2. Linux 배포 설정 변경
wsl --set-version Ubuntu-20.04 2
(리눅스 커널 설치가 필요할 수 있다.)
(https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

3. wsl2를 기본으로 설정
wsl --set-default-version 2
```

설치 확인.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/daead382-7e86-4095-afd6-1bc61bee7397/Untitled.png)
</details>
 
