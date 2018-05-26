---
layout: post
title: 2018 상반기 개발자로서 잘 보내고 있는가?
category: Diary
---

서른 살 상반기를 보내고 있는 나는 개발자로서 성장하고 있다고 생각하는가? 

문득 이런 생각이 들었다. 벌써 2018년도 반이나 흘러갔는데 ~~나이 먹을수록 왜 이리 시간이 빨리 가나..~~ 과연 잘 하고 있는 건가? 

요즘 들어 뭔가 정체되어 있다는 생각도 많이 들고 좀 지쳐있다 보니 이런 생각을 자주 하게 되는 거 같다. 

그래서 요새 가장 많이 생각하는 주제에 대해서 글을 쓰면서 생각을 정리해보고자 한다.

# 성장하기 위해서 뭘 했고 뭘 하고 있는가?

성장을 위해서 현 직장에 이직했는데 이직한 이유는 서비스를 만들어서(Front-end + Back-end) AWS 환경에서 원활하게 서비스를 운영해보고 싶었기 때문이다. 그리고 그걸 하기 위해서 정말 바쁜 나날을 보냈고 지금은 작은 팀을 이끄는 팀장이 되었다.

## 공부하고 삽질하고 적용하고...

전 직장에서 쓰던 Java + Spring이 아닌 Node.js라는걸 공부해서 실무에서 쓰기 시작했고, Node.js 고른 이유는 전 직장에서 썼던 Java보다 Node.js + npm를 이용하면 좀 더 린하게 MVP(Minimum Viable Product)을 빨리 뽑아낼 수 있는 장점 때문이었다.

Front-end는 Vue.js + Vuex + Vue Router를 공부해서 화면을 만들기 시작했고, Vue.js를 고른 이유는 [Single File Components](https://vuejs.org/v2/guide/single-file-components.html) 때문이다. SPA를 하면서 Component 단위로 쪼개야 하는데 그걸 가장 편리하게 할 수 있는 Framework가 나에게는 Vue.js 였고, 만약에 웹 퍼블리싱만 해주는분이 생겼다고 가정했을 때 그분이 해준 마크업들을 template 부분에 그대로 복붙해서 쓸 수 있다는 장점 때문이었다.

Back-end를 만들기 위해서 Express.js를 공부해서 API를 만들기 시작했다. Express.js를 고른 이유는 Node.js web application framework 중에서 가장 Minimalist Web Framework로써 무언가를 뗐다 붙었다 하는 게 나한테는 굉장히 편하다는 장점 때문이었다.

AWS에 서비스를 띄우기 위해서 남들이 취미 생활하는 비용을 AWS에 써보자 ~~이러다가 한번 요금폭탄 맞았다.~~ 라는 생각으로 내 돈을 써가면서 AWS를 학습했다. EC2, S3, Cloud Front, Route 53, RDS, Elastic Beanstalk, Lambda, API Gateway, AWS IoT 모든 게 처음인 상황에서 열심히 이것저것 삽질하면서 서비스에 적용했다.

매일 새벽이 되면 돌아야 하는 Cron 작업을 AWS Lambda + Cloud Watch Events를 이용해서 보낼 수 있게 삽질을 하면서 적용했고, 그걸 좀 더 편하게 사용하고 싶어서 예전에 공부하고 [발표](http://slides.com/leoheo/aws-cron-feat-apex)했던 [apex](https://github.com/apex/apex)를 적용했다.

AWS IoT + API Gateway를 이용해서 IoT 기기들을 제어하는 API를 만들기 위해서 하드웨어 만드는 팀원과 협업하면서 열심히 삽집을 했다. 이거에 대해서는 [qitta](https://qiita.com/ma2shita/items/d558929c58d497a622e3)에 설명이 잘 되어 있다.

1년 이상 돌릴 EC2 instance를 On-premise로 돌리는 실수가 있어서 [RI](https://gist.github.com/LeoHeo/b7d43d0a51fb555d08d5900052b2ca3c)에 대해서 알아보고 적용하고 S3 + Cloud Front + Route 53 + Elastic Beanstalk + RDS 연동시키기 위해서 삽질하고 적용했다.

## 좀 더 성장 하기 위해서

하지만 이런 상태에서 좀 부족하다고 느낀 게 그때그때 삽질했던 걸 기억이 아니라 기록으로 남기고 싶었다. 그래서 뭘 할 때마다 기록을 남기는 버릇이 있는 사람들이 엄청 부러웠다. 그래서 나도 기록하는 습관을 만들면 좀 더 성장할 거 같다는 생각이 들었고 그래서 [글또 - 글쓰는 또라이 - 글쓰는 또라이가 세상을 바꾼다](https://www.facebook.com/groups/375431516259701/){:target="_blank"}를 가입했다. 나는 주로 삽질기나 tips 위주의 글을 썼고 그로 인해서 뭘 할 때 기록하는 습관이 예전보다는 많이 좋아졌다. 글또를 가입하고 반 정도 지난 이 시점 평가를 해보자면 정말 2018년에 한 일 중에 Top 5 안에 들 정도로 잘한 일이다. 옆에서 계속 좋은 자극제를 주시는 많은 분들이 있었기에 계속해서 글쓰기를 이어올 수 있지 않았나 싶다.

요즘에는 Java + Spring을 다시 공부하고 있어서 그에 대한 글을 쓰고 있다. 현 직장으로 오면서 Java + Spring보다는 Node.js를 주로 사용하고 있는데 Java + Spring 공부를 하지 않으면 예전 기억마저도 까먹을 거 같아서 공부를 시작했다.

[spring-tips](https://github.com/LeoHeo/spring-tips)라는 repository를 만들어서 [토이프로젝트](https://github.com/LeoHeo/collect) 하면서 삽질했던걸 기록하고 있고 [bookmark](https://github.com/LeoHeo/bookmark)로 개발공부할때 많이쓰는곳들을 남기는 습관이 생겼다. 

## 결론

성장하기 위해서 뭘 했고 뭘 하고 있는가? 라는 주제로 글을 쓰면서 생각을 한번 정리해봤다.

![](https://image.slidesharecdn.com/freedomresponsibilityculture-150609081022-lva1-app6892/95/-25-638.jpg?cb=1439617172)
<br>
[출처 - 넷플릭스의 문화 : 자유와 책임 (한국어 번역본)](https://www.slideshare.net/watchncompass/freedom-responsibility-culture)


위 글은 평소에 내가 꿈꾸는 일터의 모습이다. 멋진 동료를 만나기 위해서는 나부터가 남들한테 **멋진 동료가 될 수 있는가?** 라는 생각을 하게 된다.