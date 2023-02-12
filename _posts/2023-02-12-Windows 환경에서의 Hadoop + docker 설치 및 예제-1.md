---
title : Windows 환경에서의 Hadoop + docker 설치 및 예제-1
date : 2023-02-12 13:48:00 +09:00
categories : [Hadoop]
tags : [Hadoop, 분산처리, docker, Windows] 
---

# Docker
---
Container라는 독립된 환경에서 개발자들이 개발에 집중하고, 해당 Container를 통채로 image로 만들 수 있도록 하여 어느 시스템에서든 편리하게 구동 가능하도록 해주는 오픈소스 플랫폼

요약하자면, 편하게 돌릴 수 있는 가상환경 정도로 볼 수 있다.


- 이를 활용하면 Hadoop의 NameNode와 DataNode의 생성이 편리해질 수 있다.



## docker 설치

https://docs.docker.com/desktop/install/windows-install/

설치 과정은 전부 기본으로 진행해도 됩니다


## CentOS image 다운로드

### CentOS(Community ENTerprise Operating System)

- ****무료 (아주 중요함)****

- 오픈 소스 리눅스 배포판

- Red Hat Enterprise Linux(RHEL)와 호환

- 커뮤니티가 잘 형성되어 있음(참고할 자료가 많음)


CentOS의 image를 사용하기 위해서는 cmd를 실행 시킨 뒤에

```cmd
docker run -it --name hadoop-base centos:7
```

를 입력해주거나, Docker Desktop에서 우상단에 있는 검색창에서 image 검색을 활용하여 run 해주면 됩니다.

![설치과정1](https://user-images.githubusercontent.com/50907018/218299597-803afd4f-d471-49f9-aabd-850fdf921982.png)


다만 이후 cmd에서 계속 진행할 것이므로 cmd를 통한 docker run을 권장합니다

![설치과정2](https://user-images.githubusercontent.com/50907018/218299661-838d1258-e588-46fa-a448-eafd510e7cc1.png)
> 설치 이후 docker image ls 명령어를 실행시킨 뒤, centos를 확인할 수 있으면 설치가 잘 된 것입니다

run 명령어를 통해 실행시킬 경우, 바로 해당 container의 터미널에 진입하게 됩니다.

```cmd
/* CentOS container 내부 터미널 */
yum update
yum install wget -y
yum install vim -y
yum install openssh-server openssh-clients openssh-askpass -y
yum install java-1.8.0-openjdk-devel.x86_64 -y
```
> 위 명령어 실행합니다

### wget 

- 가장 널리 사용되는 인터넷 프로토콜인 HTTP, HTTPS 및 FTP을 사용하여 파일을 받아오는 SW 패키지

### vim

- 터미널 내부에서 문서 편집이 가능하도록 해주는 문서 편집기


### openssh 관련

- 안전하지 않은 네트워크를 통해 시스템에 안전하고 편리한 원격 액세스를 제공하는 강력한 도구

### java

- Hadoop 자체가 java 기반

```cmd
ssh-keygen -t rsa -P '' -f ~/.ssh/id_dsa
cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
 
ssh-keygen -f /etc/ssh/ssh_host_rsa_key -t rsa -N ""
ssh-keygen -f /etc/ssh/ssh_host_ecdsa_key -t ecdsa -N ""
ssh-keygen -f /etc/ssh/ssh_host_ed25519_key -t ed25519 -N "" 
```
> 위의 keygen을 위한 명령어를 실행합니다

### ssh-keygen

- -t : key의 타입을 의미

- -f : 해당 key의 경로

- -N : Password

- -P : Password, 빈칸을 줄 경우 비밀번호 X

> -N은 Private Key가 암호화되지 않음
> 
> -P는 Private Key가 암호화됨 


```cmd
readlink -f /usr/bin/javac
 
vim ~/.bashrc
```
> 명령어 실행합니다

![설치과정3](https://user-images.githubusercontent.com/50907018/218306866-788af876-17c9-4570-b6a4-55430f954d5b.png)

여기서 readlink의 결과에서 bin 디렉토리 이전까지 경로를 기록해둡니다.

이후 vim 명령어를 실행할 경우

vim 에디터 편집창으로 들어가게 됩니다.

### vim
- vim 에디터로 편집하는 명령어


![설치과정4](https://user-images.githubusercontent.com/50907018/218306934-ea3b8dc2-bad4-4fbe-a8de-b3803df1324f.png)

### 간단한 Vim 사용 방법

- 현재 위 이미지는 [일반 모드] 상태입니다

- [Insert 모드] 상태가 되어야 편집이 가능해집니다.

- [Insert 모드] 는 i 를 입력하면 진입합니다

![설치과정5](https://user-images.githubusercontent.com/50907018/218307097-21bdb85c-7831-4693-a9ca-6a5a87bb6a48.png)

- 위 상태에서 화살표를 통하여 수정할 위치에 도달한 뒤에, 일반 편집기처럼 수정을 끝내고 ESC를 통해 다시 [일반 모드]로 돌아오면 됩니다.

- Windows의 경우에는 편집기 내부에서 **마우스 우클릭을 하면 클립보드에 저장된 내용을 붙여넣기** 할 수 있습니다

- [일반 모드] 에서 :wq 를 입력하면 저장하고 에디터에서 나갈 수 있습니다.



[좀 더 많은 정보](https://nolboo.kim/blog/2016/11/15/vim-for-beginner/)




```cmd
mkdir /hadoop_home
cd /hadoop_home
 
wget https://archive.apache.org/dist/hadoop/common/hadoop-2.7.7/hadoop-2.7.7.tar.gz
 
tar -xvzf hadoop-2.7.7.tar.gz
```
> hadoop을 설치합니다

```cmd
vim ~/.bashrc
```
> 다시 vim 에디터로 환경변수를 설정해줍니다.

![설치과정6](https://user-images.githubusercontent.com/50907018/218307416-6d8f26d6-03ae-472a-8a55-0d01b3b5e429.png)

> 위 이미지처럼 환경변수를 추가해줍니다.

```cmd
export HADOOP_HOME=/hadoop_home/hadoop-2.7.7
export HADOOP_CONFIG_HOME=$HADOOP_HOME/etc/hadoop
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
 
/usr/sbin/sshd
```
> 환경변수 설정

```cmd
source ~/.bashrc
```
> 위 명령어 실행합니다


### source

- 현재 쉘 환경에서 쉘 스크립트 파일을 실행하는 명령어

- bashrc에 설정한 환경 변수들을 현재 환경에 적용한다고 보면 됨 


``` cmd
cd $HADOOP_CONFIG_HOME

cp mapred-site.xml.template mapred-site.xml
```
> 명령어 실행

```cmd
vim core-site.xml
```

![설치과정7](https://user-images.githubusercontent.com/50907018/218308819-d237bacb-ed22-4701-96e8-1bcb4f0199f6.png)

```xml
<!-- core-site.xml -->
<configuration>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/hadoop_home/temp</value>
    </property>
 
    <property>
        <name>fs.default.name</name>
        <value>hdfs://nn:9000</value>
        <final>true</final>
    </property>
</configuration>
```
> 해당 부분 추가

```cmd
vim hdfs-site.xml
```
> 명령어 실행

![설치과정8](https://user-images.githubusercontent.com/50907018/218308942-2fadb93f-bf54-485c-844b-56884648e2de.png)

```xml
<!-- hdfs-site.xml -->
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>2</value>
        <final>true</final>
    </property>
 
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/hadoop_home/namenode_dir</value>
        <final>true</final>
    </property>
 
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>/hadoop_home/datanode_dir</value>
        <final>true</final>
    </property>
</configuration>
```
> 해당 부분 추가 

```cmd
mkdir /hadoop_home/temp

mkdir /hadoop_home/namenode_dir

mkdir /hadoop_home/datanode_dir
```
> 명령어 실행

```cmd
vim mapred-site.xml
```
> 명령어 실행

![설치과정9](https://user-images.githubusercontent.com/50907018/218309078-b376a4b4-c7e4-42ab-8c85-dfb297fe1b9f.png)

```xml
<!-- hdfs-site.xml -->
<configuration>
    <property>
        <name>mapred.job.tracker</name>
        <value>nn:9001</value>
    </property>
</configuration>
```
> 해당 부분 추가

```cmd
hadoop namenode -format
exit
```
> 명령어 실행

### hadoop namenode -format

- namenode(Master) 포맷 시키는 명령어

- 내부 데이터를 전부 삭제시키고 새롭게 서비스를 시작하기 위한 준비


```cmd
/* Windows cmd */

docker commit hadoop-base centos:hadoop
```
> 이미지 생성을 위한 명령어 실행

- docker commit [container] [image name] 

- 만약 container name이 hadoop-base가 아니라면 바꿔주시면 됩니다.

- 명령어 docker ps 로 현재 실행중인 container 확인 가능



## docker image 생성 완료

---

image가 재사용을 위한 방법이기 때문에 지금까지 설정한 부분들이 hadoop 설정에서 반복될 수 있는 구간이라는 것을 생각해볼 수 있습니다
















