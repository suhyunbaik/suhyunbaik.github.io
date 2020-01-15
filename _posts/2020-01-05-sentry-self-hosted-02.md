---
layout: post
title: Sentry를 Ubuntu 18.04(Bionic) 에 self-hosted 형태로 구축하기 - 2 추가 설정하기
tags: [Sentry, Docker]
categories: [DevOps]
image: images/thumbnails/sentry_and_selfhosted.png

---

![sentry](/images/posts/sentry-logo.png)  

1. 메일 설정

    

##### 1. 메일 설정

센트리에서 멤버를 초대하거나, 알람을 받으려면 메일을 설정해야 한다. 센트리를 처음 접속할 경우에는 브라우저에서 바로 설정할 수 있다. 

![sentry](/images/posts/sentry-setting.png)  
나는 브라우저에서 설정하지 않고 `config.yml`에서 직접 수정했다.

`config.yml`

```yaml
###############
# Mail Server #
###############

mail.backend: 'smtp'  # Use dummy if you want to disable email entirely
mail.host: 'smtp.gmail.com'
mail.port: 587
mail.username: 'email'
mail.password: 'password'
mail.use-tls: True
# The email address to send on behalf of
mail.from: 'email'
# If you'd like to configure email replies, enable this.
# mail.enable-replies: true

# When email-replies are enabled, this value is used in the Reply-To header
# mail.reply-hostname: ''

# If you're using mailgun for inbound mail, set your API key and configure a
# route to forward to /api/hooks/mailgun/inbound/
# Also don't forget to set `mail.enable-replies: true` above.
# mail.mailgun-api-key: ''
```

메일 관련 부분을 찾아서 수정해주면 된다.