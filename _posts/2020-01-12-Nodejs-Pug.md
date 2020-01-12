---
layout: post
title: 익스프레스 템플릿(pug)
tags: [pug, node.js]
---

#### Pug 대하여

pug는 view engine이며, template언어 이고, html 파일들을 더 보기 편하고 쉽게 이해하게 만들어준다.

pug, express에는 view 파일들의 위치에 관한 기본 설정이 존재 한다.

만약 그 설정을 바꾸고 싶다면 expressjs의 공식문서에 나온 것 처럼 view 설정들을 바꾸면 된다.

![pugseting](/assets/img/pexels/pugseting.png)

그렇다면 이 template을 웹사이트에서 보여주려면 어떤 방식으로 작업 해야할까?

Controllers 폴더에 있는 컨트롤러 파일들 중에 res.send를 res.render를 사용해서 해결 할 수 있다.

![render](/assets/img/pexels/render.png)

그럼 이러한 함수가 view 폴더에서 파일명이 home이고, 확장자가 pug인 템플렛 파일을 찾은 후에 보여준다.

확장자 pug는 이전에 우리가 view engine에서 설정 해줬다.

pug가 html에 보다 좋고 편리한 이유는 반복되는 작업은 따로 만들어 놓고 전체의 html을 유지보수 할 수 있다는 것이다.

지금 하는 코드에서 우리는 main.pug를 중심으로 하고, 거기에 header.pug 그리고 foorter.pug 두개를 따로 만들어서 반복적으로 쓰며 수정이 필요하다면 저 세 곳만 고쳐서 전체 html 페이지를 바꿔 줄 수 있다.

즉, 우리가 home 화면에 접근하게 된다면 기본적인 틀(전반적으로 화면 이동이 이뤄져도 반복적으로 보이는 것들)을 보이게 된다. 그럼 main.pug가 실행되고 그 안에 html 기본 틀 및 header.pug, foorter.pug가 있고, block content를 통해서 다른 pug의 내용이 적용 된다.

예를들어 첫 홈 화면에서 회원가입으로 이동 했다면 main.pug에 작성된

    main
    	block content

덕분에 join.pug의 화면이 보이게 된다.

그럼 join.pug 화면에서는 우리가 꼭 반복적으로 설정해줬던 main.pug, header.pug, foorter.pug 파일들이 보이게 되고(여기에 작성한 코드들이 보인다는 말), join.pug에선

    extends layouts/main

    block content
         p Join

이걸 통해 main.pug에 block content 위치에 p태그 Join이 실행 되어있다.

위에 언급 했듯이 우리는 header.pug, foorter.pug도 따로 만들어서 main.pug에서 쓴다고 했는데, 쓰는 방법은 사용 하고 싶은 곳에 include로 시작해서 경로를 지정하면 된다.

    include ../partials/header 이러한 방식!

또한 pug 속에서 javascript를 사용하기 위해서는 #{} 이 안에 코드를 작성하면 된다. 예를들어

    span.footer__text #{siteName} #{new Date().getFullYear()} &copy;
    이러한 방식!

그리고 우리는 컨트롤러에 있는 정보를 템플렛에 추가하는 법도 있는데 하나의 템플렛에만 추가하거나, 전체 템플렛에 추가 할 수 있다.

그중 먼저 전체적으로 템플렛에 추가하는 방법은 미들웨어를 이용하는 방법이 있다.

![middlewaresjs](/assets/img/pexels/middlewaresjs.png)

이런식으로 만든다.

여기서

    res.locals.siteName = "WeTube"; 는 footer.pug에서 #{siteName} 위치에 WeTube가 들어가게 된다.

이 이미지와 같이 코딩하면 된다.

![footer](/assets/img/pexels/footer.png)

또 우리는 위에 보이는 미들웨어에서

    res.locals.routes = routes;

이것도 코딩을 작업 했는데, 밑에 이미지 처럼 사용하게 된다.

![header](/assets/img/pexels/header.png)

보면 a(href=routes.join) Join 이러한 방식으로 사용 된걸 알 수 있다.

마지막으로 하나의 템플릿에만 변수를 추가 하려면 Controller.js에서 #{쓰고싶은이름적기}를 작성하고 템플렛에 적용 시켜주면 된다.

밑에 이미지와 같다.

![pagetitle](/assets/img/pexels/pagetitle.png)

res.render("home", { pageTitle: "Home" });

이곳에서 이뤄지는건데, 첫번재 "home" 인자는 템플렛이고, 두번째 인자 { pageTitle: "Home" }는 템플렛에 추가할 정보가 담긴 것이다.

고로 템플렛에 작성해서 사용하는 법은 밑에 이미지 처럼 사용하면 된다.

![pagetitle2](/assets/img/pexels/pagetitle2.png)
