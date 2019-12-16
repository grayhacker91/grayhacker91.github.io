---
title: "React Component"
date: 2019-12-04
categories:
  - React
tags:
  - js
  - front-end
---

## 함수와 컴포넌트

다른 언어를 포함해서 자바스크립트의 함수는 코드를 명확하고 재사용 가능하도록 만든다.
그리고 함수의 또 하나의 가치는 함수 안에서 다른 함수를 호출할 수 있다는 것이다.
이 점을 생각하고 전의 내용의 render메소드 살펴보자.

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

container내용 안에 다른 내용도 보여주려고 할 때, 아래와 같이 추가할 것이다.
(h1태그들을 div태그로 감싼 것은 리액트에서는 인접한 여러 엘리먼트를 출력할 수 없기 때문이다.)

```html
<div id="container"></div>
<script type="text/babel">
  var destination = document.querySelector("#container");
  ReactDOM.render(
    <div>
      <h1>Pocket Monster</h1>
      <h1>Larva</h1>
      <h1>Digital Monster</h1>
    </div>,
    destination
  );
</script>
```

그리고 여기서 h1태그가 아닌 h3태그로, 그리고 h3태그 안 내용을 이탤릭체로 변경하고 싶을 때마다, 일일이 변경해주어야 할 것이다.
위와 같이 일일이 해야하는 비효율적인 작업을 함수와 같이 보다 쉽게 해줄수 있는 것이 컴포넌트라고 생각하면 될 것이다.

## 리액트 컴포넌트 시작하기

리액트의 컴포넌트는 JSX를 통해서 HTML 엘리먼트를 출력하고 재사용 가능한 자바스크립트 덩어리이다.
좀 더 알기쉽게 아래의 간단한 render메소트를 컴포넌트로 작성해보자.

```javascript
ReactDOM.render(
  <div>
    <p>Hello, React</p>
  </div>,
  document.querySelector("#container")
)
```

컴포넌트로 만드는 방법 중 하나인 React.Component를 사용한다.(책에서는 React.createClass를 사용했는데, 이 방법은 16버전부터 사라졌다.)

```javascript
class Hello extends React.Component {
  render() {
    return (
      <p>Hello, Component</p>
    );
  }
};
```

먼저 Hello라는 컴포넌트를 생성하고 이를 동작하게 할 속성중 하나인 render를 추가한다. 컴포넌트 안의 render메소드도 ReactDOM.render메소드와 똑같이 JSX처리하기 때문에 Hello, Component 텍스트를 리턴하게 만든다.
그리고 만들어진 컴포넌트를 render메소드에서 사용하면 된다.

```javascript
ReactDOM.render(
  <Hello/>,
  document.querySelector("#container")
)
```

지금까지는 텍스트를 출력만 했다면, 속성(함수에서는 파라미터라고 지칭)을 전달해서 그에 맞게 동작하도록 해보자.
먼저 컴포넌트에서 속성을 정의(여기서는 target)하고, 호출하는 곳에서 속성을 추가하도록 하자.

```javascript
class Hello extends React.Component {
  render() {
    return (
      <p>Hello, {this.props.target}</p>
    );
  }
};
ReactDOM.render(
  <div>
    <Hello target="world!"/>
    <Hello target="PocketMon"/>
    <Hello target="Larva"/>
  </div>,
  document.querySelector("#container")
)
```

끝으로 컴포넌트에서도 자식을 가질 수 있다. 아래와 같이 Hello의 컴포넌트는 p태그를 자식으로 가지고 있다. 그리고 컴포넌트 안 자식들은 this.props.children을 통해서 접근이 가능하다.

```javascript
<Hello target="world!">
  <p>Charmander</p>
</Hello>
```

예시)

```javascript
class Button extends React.Component {
  render() {
    return (
      <div>
        <button>{this.props.children}</button>
      </div>
    );
  }
};
ReactDOM.render(
  <Button>
    <p>push</p>
  </Button>,
  document.querySelector("#container")
)
```

this.pros.children 속성은 자식이 여러 개라면 배열로, 하나라면 단일 컴포넌트로 전달해준다.

지금까지 작성된 전체 예제

```html
<!DOCTYPE html>
<html>
<head>
  <title>React! React! React!</title>
  <script src="https://unpkg.com/react@16.12.0/umd/react.development.js"></script>
  <script src="https://unpkg.com/react-dom@16.12.0/umd/react-dom.development.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.23/browser.min.js"></script>
</head>
<body>
  <div id="container"></div>
  <script type="text/babel">
    class Button extends React.Component {
      render() {
        return (
          <div>
            <button>{this.props.children}</button>
          </div>
        );
      }
    };
    class Hello extends React.Component {
      render() {
        return (
          <p>Hello, {this.props.target}</p>
        );
      }
    };
    ReactDOM.render(
      <div>
        <Hello target="world!"/>
        <Hello target="PocketMon"/>
        <Hello target="Larva"/>
        <Button>
          <p>push</p>
        </Button>
      </div>,
      document.querySelector("#container")
    )
  </script>
</body>
</html>
```
