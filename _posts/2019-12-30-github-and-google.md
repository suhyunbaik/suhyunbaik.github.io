---
layout: default
title: 깃허브 블로그가 구글에 검색되게 하기
tags: [blog]
categories: [blog]
---

깃허브 블로그가 구글 검색 결과에 노출되려면 추가 작업을 해야 한다.


#### 1.구글 웹마스터에 블로그 등록

![github](/images/posts/github-01.png)  
구글계정으로 웹마스터에 접속해 속성을 추가 한다. 

블로그 소유자임을 인증해야 한다. 구글에서 제공하는 HTML을 다운받아 깃허브 블로그 최상단 디렉토리에 넣고 원격저장소에 푸시한다.  
![github](/images/posts/github-02.png)  

구글 웹마스터로 돌아와 인증을 완료한다.



#### 2. sitemap.xml 파일 생성

Jekyll에 sitemap.xml을 자동으로 생성하는 플러그인이 있다. `_config.yml`에 해당 코드를 추가한다.

```yaml
plugins:
	- jekyll-sitemap
```

플러그인을 설치 한 후, `sitemap.xml`을 확인한다. `Jekyll serve` 명령어로 로컬 서버를 실행한 뒤 `127.0.0.1:4000/sitemap.xml` 에 접속해서 내용을 확인한다.



#### 3. Robots.txt 파일 생성

robots.txt에 sitemap.xml 위치를 등록하면 검색엔진 크롤러들이 홈페이지를 크롤링을 할 때 참고한다.

블로그 최상단에 `robots.txt` 파일을 생성한다. 아래 코드를 내용으로 넣는다.

```
User-agent: *
Allow: /
Sitemap: http://[github사용자명].github.io/sitemap.xml
```


#### 4.구글 웹마스터에 sitemap.xml 등록 

구글 웹마스터에 접속해 `sitemap.xml`을 등록해야 한다. 

![github](/images/posts/github-04.png)  


