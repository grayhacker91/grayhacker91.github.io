---
title: "React 시작"
date: 2019-12-02
categories:
  - React
tags:
  - js
  - front-end
---

## JSX다루기

이전 글에서 나왔듯이 리액트는 JSX를 이용해서 자바스크립트와 HTML태그를 합쳐 사용자 인터페이스 요소와 기능을 정의할 수 있다. 하지만 브라우저는 JSX를 모르기 때문에, 알 수 있도록 평범한 자바스크립트로 변환시켜야 한다.
이를 해결하기 위한 두 가지 방법으로

1. Webpack, Node등 빌드 툴을 이용해서 개발환경을 구축하고, 빌드를 수행할 때마다 JSX를 자바스크립트로 변환하는 방법
2. JSX를 자동 변환하는 자바스크립트 라이브러리를 사용하는 방법

1번 방법은 웹앱을 제작할 때 대표적으로 사용하는 방법이다. 처음에 다소 개발환경을 세팅하고 설정을 지정하는 등 복잡할 수 있으나, JSX변환과 더불어 여러가지 웹앱의 관리에 필요한 많은 기능을 제공한다.
2번 방법은 개발환경에 소요되는 시간을 절약하고 코드 작성에 집중하는 직접적인 방법이다.
(여기서는 예제 작성 및 테스트를 위해서 2번 방법을 택)

예제코드 중 render 메소드에 대해서 살펴보자
ReactDOM.render 메소드는 두 개의 인자를 받는다.

1. 화면에 출력할 HTML(JSX)
2. JSX를 렌더링한 DOM위치

```html
<script type="text/babel">
    ReactDOM.render(
      <h1>Pocket Monster</h1>,
      document.body
    );
</script>
```

예제에서는 h1태그가 첫 번째 인자이고, 두 번째 인자로 document.body를 작성해 해당 위치에 삽입했다. 하지만, body태그에 바로 집어 넣는 것은 여러 문제가 발생할 소지가 높기에 아래의 방법과 같이 별도의 새로운 엘리먼트를 만들어서 이를 루트 엘리먼트로 사용하기로 한다.

```html
<div id="container"></div>
<script type="text/babel">
  var destination = document.querySelector("#container");
  ReactDOM.render(
    <h1>Pocket Monster</h1>,
    destination
  );
</script>
```

스타일 시트까지 적용된 전체 예제

```html
<!DOCTYPE html>
<html>
<head>
  <title>React! React! React!</title>
  <script src="https://unpkg.com/react@16.12.0/umd/react.development.js"></script>
  <script src="https://unpkg.com/react-dom@16.12.0/umd/react-dom.development.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.23/browser.min.js"></script>
  <style>
    #container {
      padding:50px;
      background-color:#EEE;
    }
    #container h1{
      font-size:144px;
      font-family:sans-serif;
      color:#0080a8;
    }
  </style>
</head>
<body>
  <div id="container"></div>
  <script type="text/babel">
    var destination = document.querySelector("#container");
    ReactDOM.render(
      <h1>Pocket Monster</h1>,
      destination
    );
  </script>
</body>
</html>
```

화면
![예제1 화면](/images/react_example1.JPG)
