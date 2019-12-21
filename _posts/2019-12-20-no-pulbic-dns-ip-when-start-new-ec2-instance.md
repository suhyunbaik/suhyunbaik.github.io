---
layout: post
title: EC2 인스턴스 생성시 Public DNS, IP 미할당 
tags: [AWS, EC2, troubleshooting]
---
인스턴스를 새로 생성했는데 Public DNS가 계속 미할당됐다. 인스턴스 종료 및 생성 과정을 계속 반복했는데 원인은 다른곳에 있었다.

Public DNS가 미할당 상태가 되는 원인 몇가지를 정리했다.

#### 1. VPC에 DNS hostname 기능이 no 로 되어있다.
   해결방법 : 인스턴스 상세 패널에서 `VPC` 클릭> VPC 패널이 열리면 `서브넷` 메뉴 선택 > 수정하고 싶은 서브넷을 우클릭하면 `자동할당 IP 설정 수정(Modify Auto-Assign Public IP)` 메뉴가 뜸 > 해당 설정을 수정

#### 2. 인스턴스 생성시 Step3: Configure Instance Details 화면에서 Public DNS 항목이 disabled 됐다.
   해결방법 > step3에서 DNS 항목을 Enable로 설정


