---
layout: page
title: scale in 할때 5xx 에러 발생
tags: [DevOps, 5xx]
categories: [DevOps]
---
현재 로드밸런서에서 5xx 에러가 발생할 때 마다 메일이 오도록 설정해서 사용하고 있다. 그런데 스케일 인 할 때 마다 5xx 에러가 발생하는 문제가 있었다. 나는 처음에 커넥션 드레인이 제대로 이뤄지지 않아서 에러가 발생한다고 생각했다. 현재 서버가 사용하는 로드밸런서는 `OpsWorks`에서 제공하는 `Classic load balancer`이다. 그래서 드레인이 제대로 안됐을거라 추측했으나 그건 아니였고 설정이 이미 다 잘 되어 있었다. 두번째로 짐작한 원인은 `uWSGI` 의 짧은 `harakiri` 시간이다. 드레인 시간보다 `harakiri` 시간이 짧게 설정되어있다면 먼저 커넥션을 끊기 시작할테니 connection permanently closed 를 뱉으면서 5xx 에러를 일으킬 수 있다고 생각했다. 그러나 uWSGI 설정 파일을 확인해보니 harakiri 시간이 드레인 시간보다 훨씬 길게 설정되어있었다. 세번째로 추측한건 API 중에 리퀘스트를 받아서 리스폰스를 되돌려줄때까지 많은 시간이 소요되는 API 가 다수 존재하고 소요 시간이 드레인 시간보다 더 오래 걸리기 때문에 5xx 에러를 일으키는 것이다. 이 부분은 확인하기 힘들었다. 왜냐면 APM 서비스를 사용하고 있지 않아서 정확히 어떤 API 가 시간이 오래 걸리는 지 알 수 없었기 때문이다. 

결국 문제는 해결했으나 원인은 의외의 곳에 있었다. ec2에 엘라스틱 서치를 설치해서 사용하고 있는데 평소에도 cpu 사용률이 90%가 넘은 상태였다. 해당 인스턴스를 스케일 업을 하니까 더이상 에러가 나지 않았다. 엘라스틱 서치 인스턴스와 웹서버 인스턴스 사이에 커넥션이 종료되지 않았는데 인스턴스가 종료되면서 커넥션이 끊기니까 5xx 에러가 났던 것이다. 다음부터는 에러가 나면 cpu 사용률이나 먼저 확인해봐야겠다. 




