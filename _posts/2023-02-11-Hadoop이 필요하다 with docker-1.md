---
title : Hadoop을 배워보기-1
date : 2023-02-10 11:05:00 +09:00
categories : [Hadoop]
tags : [Hadoop, 분산처리, docker] 
---

# Hadoop

---

![코끼리를 냉장고에 넣는 방법!!!](https://t1.daumcdn.net/cfile/tistory/21298040572079330D)

> 냉장고에 코끼리를 넣는 방법은?

![하둡 10년, 빅데이터의 역사를 이끌다 - Byline Network](https://byline65dotcom.files.wordpress.com/2016/01/apache-hadoop.jpg)

> 그건 바로 hadoop이다.
> 살아있는 코끼리를 토막내서 넣고, 다시 빼낼 때는 살아있는 상태의 코끼리로 꺼내는 방법이라고 할 수 있다

- 범용 하드웨어 클러스터

- 대규모 데이터셋 분산 저장 및 처리

- 오픈 소스 SW Framework

---
### 범용 하드웨어 클러스터(A cluster of commodity hardware)

- 데이터 처리, 저장과 같은 Task에 사용되는 상호 연결된 시스템 또는 서버 그룹

- 범용 하드웨어 라는 의미는 특정 작업을 위해 특별히 설계된게 아닌, 상대적으로 저렴한 기성 제품이라는 의미

- 저렴하지만 많은 컴퓨터로 높은 수준의 성능과 신뢰성

- 빅데이터 처리

- 클라우드 컴퓨팅

- 확장 가능한 솔루션

---



Hadoop 은 대규모 조직에서 생성되는 막대한 양의 데이터를 처리하고 데이터 집약적인 애플리케이션을 위한 비용 효율적이고 확장 가능한 솔루션을 제공하도록 설계되었습니다.  

### HDFS(Hadoop Distributed File System)

- 확장성
- 안정성
- 내결함성

- master-slave architecture에서 작동
> NameNode - Master
> DataNode - Slave

NameNode는 file system namespace를 관리하고 시스템에 저장된 파일 및 디렉터리에 대한 **메타데이터 정보** 유지 관리

DataNode는 실제 데이터를 저장하는 역할
  

데이터가 HDFS에 기록되면 블록으로 분할되며, 일반적으로 128MB 또는 256MB 크기이며, 각 블록은 중복성 및 내결함성을 위해 여러 데이터 노드에 저장됩니다. 이를 통해 HDFS는 개별 노드의 장애를 허용하면서도 데이터의 접근성과 신뢰성을 유지할 수 있습니다.  
  

### YARN

- 처리 작업을 작고 독립적으로 나눔

- 병렬 처리

- 클러스터 노드에서 병렬으로 실행


### 왜 Hadoop을 사용하는가?

- 오픈 소스

- 경제적, 확장 가능

 > Facebook, Yahoo 및 eBay 같은 대기업에서도 사용 중
 -   _검색_ – Yahoo, Amazon, Zvents
-   _로그 처리_ – Facebook, Yahoo
-   _데이터 웨어하우스_ – Facebook, AOL
-   _비디오 및 이미지 분석_ – New York Times, Eyealike


## Hadoop이 권장되지 않는 상황

-   _데이터에 대한 빠른 접근이 필요한 상황_ : 데이터의 작은 부분에 대한 빠른 액세스 필요

-   _다중 데이터 수정_ : 데이터 수정이 아닌 **데이터 읽기에 주로 관심이 있는 경우에만 Hadoop이 더 적합**합니다.

-   _작지만 많은 파일들_ : Hadoop은 이와 반대로 **파일은 적으나 큰 용량을 차지하는 상황에서 필요**합니다
  

## Hadoop이 필요한 상황

### **Hadoop-CERN 사례 연구**

- 약 1억 5천만 개의 센서가 장착되어 매초 1페타바이트의 데이터를 생산하고 있으며 데이터는 지속적으로 증가

이런 상황에서 확장성은 필수적이었고 이에 따라서 Oracle / Hadoop의 하이브리드 시스템을 구현했다.







## ML 분야의 특징

---

빅데이터를 다루는 경우, 보통 ML으로 접근하는게 가장 최선인 경우가 많다.

결국 ML을 사용하여 문제를 해결하기 위해서는 여러 인프라 구축이 필요한데, 이 중 하나가 분산 처리 시스템이고, Hadoop이 그 중에서 좋은 선택이 될 것



## 

### 참고

https://www.edureka.co/blog/what-is-hadoop/












