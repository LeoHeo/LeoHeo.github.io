---
layout: post
title: AWS ElasticBeanstalk Multi Container Docker 서울 리젼 삽질기
category: AWS
---

AWS 서울리젼에 ElasticBeanstalk Multi Container Docker가 [지원](https://aws.amazon.com/about-aws/whats-new/2018/03/aws-elastic-beanstalk-supports-docker-ruby-2_5-and-ec2-instance-types-in-additional-regions/?sc_channel=sm&sc_campaign=launch_&sc_publisher=FACEBOOK&sc_country=Global&sc_geo=GLOBAL&sc_outcome=awareness&trk=sm_Elastic_Beanstalk_d627fa4f_Support_for_Ruby_2_5_new_instance_types_and_Docker_platform_in_new_regions_FACEBOOK&sc_content=Elastic_Beanstalk_d627fa4f_Support_for_Ruby_2_5_new_instance_types_and_Docker_platform_in_new_regions&linkId=49494517)한다는 소식을 들었습니다.
그래서 한번 시도를 해봤던 삽질기를 기록하고 공유합니다.

삽질했던 순서대로 글이 진행됩니다.
향후 아랫글에서 부터는 AWS ElasticBeanstalk를 AWS eb로 줄여서 부르도록 하겠습니다.

<br />

## 시도하게 된 이유
맨 처음 시도를 하게 된 이유에는 AWS eb에 Node 8로 배포를 할려고보니
Node 8(8.9.3, 8.8.1)은 [npm5를 사용](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/concepts.platforms.html#concepts.platforms.nodejs){:target="_blank"}하게 되는데 이게 AWS eb 배포할때 `node-gyp`이슈가 있다는걸 발견했습니다.

이번에 알게된거지만 AWS eb에 Node를 배포하고 `npm install`를 하게 되면 기본적으로 root권한으로 install를 하게 되는데 node-gyp의 경우는 그렇지 않다는것입니다.

`node-gyp` 경우에는 `ec2-user(default user)`의 권한으로 install을 하는데 이럴경우 ec2-user는 `/tmp/deployment/application/node_modules/` 경로에 대한 권한이 없으므로 npm install을 할때 에러가 발생합니다.

![](http://gif-finder.com/wp-content/uploads/2016/05/Wee-Bey-Whoa-What.gif)

그래서 일부 node-gyp가 trigger되는 모듈의 경우를 생각해서 `.npmrc`에 옵션을 추가해줘야 합니다.

{% gist c4f5f7f3ae53c401681b729a85b539c4 %}

~~이걸 찾는데만 해도 시간이 꽤 소비되었습니다....~~

<br />

## AWS eb Multi Container Docker 도전!
그러던 와중에 AWS가 Seoul리젼에서 Multi Container Docker를 [지원한다는 글](https://aws.amazon.com/about-aws/whats-new/2018/03/aws-elastic-beanstalk-supports-docker-ruby-2_5-and-ec2-instance-types-in-additional-regions/?sc_channel=sm&sc_campaign=launch_&sc_publisher=FACEBOOK&sc_country=Global&sc_geo=GLOBAL&sc_outcome=awareness&trk=sm_Elastic_Beanstalk_d627fa4f_Support_for_Ruby_2_5_new_instance_types_and_Docker_platform_in_new_regions_FACEBOOK&sc_content=Elastic_Beanstalk_d627fa4f_Support_for_Ruby_2_5_new_instance_types_and_Docker_platform_in_new_regions&linkId=49494517){:target="_blank"}을 봤습니다.

> Multi Container Docker in China (Beijing), China (Ningxia), EU (Paris), Asia Pacific (Seoul), Asia Pacific (Mumbai), and South America (Sao Paulo) regions

그래서 한번 시도(얼리어답터)를 해보기로 합니다. ~~그렇게 무한 삽질의 서막이 열리고...~~

[Managing Docker & ECS Based Applications with AWS Elastic Beanstalk - DevDay Austin 2017](https://www.slideshare.net/AmazonWebServices/managing-docker-ecs-based-applications-with-aws-elastic-beanstalk-devday-austin-2017){:target="_blank"} 자료와 AWS [공식문서](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_docker_v2config.html){:target="_blank"}를 보면서 시작을 했습니다.

AWS eb에 Docker를 배포하기 위해서는 총 2가지가 필요합니다.

1. Dockerfile or docker-compose.yml 파일
2. `Dockerrun.aws.json` 파일

1번은 Docker를 쓰는 사람이면 누구나 친숙한 파일이니깐 굳이 거부감이 들지 않는데
**Dockerrun.aws.json** 이건 뭔가 처음 보는 format이라서 AWS [공식문서](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_docker_v2config.html){:target="_blank"}를 확인해봤습니다.

Dockerfile도 만들고 Dockerrun.aws.json파일도 만들었습니다.

인제 배포를 해보자!!

{% gist b0fa0eed97c5b3b8dbc54fb77e536b36 %}

eb init을 하고 절차를 밟다보면 아래와 같이 Docker를 고르는 화면이 나옵니다.

{% gist 585f59cbb1abe2e4ab585f9c137242a2 %}

로컬 터미널에서 `Docker --version`을 해보니 Docker version 17.12.0-ce라고 나오길래 1번을 골랐습니다.
~~이때는 이게 차후에 또다른 삽질을 가져올지 몰랐습니다...~~

eb init을 하고 eb create를 했는데...

```
ERROR: Solution stack Multi-container Docker 17.12.0-ce (Generic) does not appear to be valid
```

라는 에러가 나타납니다.

이게 뭔가하고 AWS 공식 문서를 찾아보는데 해당 내용에 관한 내용은 없고..
열심히 구글링을 하면서 이것저것 하다보니

[Solution stack Multi-container Docker 1.12.6 (Generic) does not appear to be valid](https://github.com/docker/for-mac/issues/1768)이라는 docker-for-mac의 github issue를 발견하게 됩니다.

기본적으로 eb-cli로 init을 하면 아래와 같은 구조로 config.yml파일이 생성됩니다.

{% gist 1d48ae857daa4056d9277c17ac4f3a6a %}

근데 저 이슈에 보면 default_platform를

{% gist e27ecfdcf44e4c8778238a515d0ddb09 %}

이렇게 수정하라고 합니다...

밑져야 본전으로 한번 default_platform을 바꿔봅니다.
Error가 나던게 갑자기 잘 작동합니다...
~~갑자기 F***로 시작하는 단어가 생각납니다....아 나의 시간이여....~~

그러고 나서 `eb create`를 했는데 생성은 잘되었지만 배포가 되지 않았습니다.

```
ERROR: No Solution Stack named '64bit Amazon Linux 2017.03 v2.7.1 running Multi-container Docker 17.03.1-ce (Generic)' found.
```

 왜 그럴까하고 또 삽질의 시간이 지나가고...
 혹시나 하고 region을 `eu-west-1`을 해보니 작동이 너무나 잘됩니다...
 ~~아... 오늘따라 F***로 시작하는 단어가 자꾸 생각나네요...~~

AWS Seoul 리젼의 경우(2018-03-21일 기준) Docker 1.12.6, Docker 1.11.2, Docker 1.9.1에 대해서는 default_platform를 바꿀 필요가 없는걸로 봐서는 나머지 ce 버젼들에 대해서는 아직 지원을 안하는거 같습니다.

<br />

## 결론

node-gyp로 인해서 AWS eb multi-container docker 서울리젼 배포하기를 해봤는데 삽질로 인해 쉽지않았습니다.
그렇지만 AWS eb multi container docker에 대해서 알게되는 계기가 되어서 좋았습니다.

