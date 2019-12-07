---
title: "React Styling"
date: 2019-12-07
categories:
  - React
tags:
  - js
  - front-end
---

## 리액트 방식의 스타일링

리액트를 이용해 웹앱을 구성하더라도, 일반적으로 쓰이는 CSS를 이용해서 스타일을 지정할 수 있다.
하지만 리액트에서는 인라인 방식의 스타일링은 선호하는데 이 방법이 비주얼 컴포넌트의 재사용성을 높이고, UI의 모양과 동작에 관련된 사항이 안전하게 담긴 작은 블랙박스인 컴포넌트로 만드는게 목적이기 때문이다.
아래의 예제 영어 단어 상자를 출력하는 컴포넌트를 이용해서 알아보자.

```javascript
class Letter extends React.Component {
  render() {
    return (
      <div>
        {this.props.children}
      </div>
    );
  }
};
```

먼저 인라인 스타일을 적용하기 위해서 해당 스타일이 모여있는 스타일 객체를 생성한다.
스타일 객체를 작성할 때, 스타일을 자바스크립트로 바꾸는 방식은 기본적으로는 그대로 쓰고, 대시로 연결된 속성은 대시를 지우고 첫 글자를 대문자로 변경하면 된다.
그 후, 해당 스타일 객체를 적용할 엘리먼트의 style속성에 지정하면 된다.
그리고 외부에서 지정한 색으로 변경하려고 할 때는 속성을 이용해서 처리하면 된다. 완성된 형태는 아래와 같다.

```javascript
class Letter extends React.Component {
  render() {
    var letterStyle = {
      padding: 10,
      margin: 10,
      backgroundColor: this.props.bgcolor,
      color: "#333",
      display: "inline-block",
      fonstSize: 32,
      textAlign: "center",
    };
    return (
      <div style={letterStyle}>
        {this.props.children}
      </div>
    );
  }
};
```

#### 접미사 'px' 생략

위 예제를 보듯이 padding과 margin, fontSize에 px를 붙이지 않았다. 이들 속성은 리액트에서 런타임시 자동으로 px를 붙여준다.
하지만 리액트에서 모든 숫자 값을 가지는 속성에 대해서 px를 붙여주지는 않는다.
자동으로 붙여주지 않는 속성들은 columnCount, fillOpacity, flex, flexGrow, zoom 등의 속성들이 있다.
(전체를 확인 할려면 리액트 공식문서 <http://shripadk.github.io/react/tips/style-props-value-px.html>에서 확인바란다.)
만약 px가 아닌 다른 접미사를 사용해되는 경우라면, 직접 접미사를 붙여주면 된다.

스타일링의 전체 예제는 아래와 같다.

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
    class Letter extends React.Component {
      render() {
        var letterStyle = {
          padding: 10,
          margin: 10,
          backgroundColor: this.props.bgcolor,
          color: "#333",
          display: "inline-block",
          fonstSize: 32,
          textAlign: "center",
        };
        return (
          <div style={letterStyle}>
            {this.props.children}
          </div>
        );
      }
    };
    ReactDOM.render(
      <div>
        <Letter bgcolor="#58b3ff">A</Letter>
        <Letter bgcolor="#ff605f">B</Letter>
        <Letter bgcolor="#ffd52e">C</Letter>
        <Letter bgcolor="#49dd8e">D</Letter>
        <Letter bgcolor="#ae99ff">E</Letter>
      </div>,
      document.querySelector("#container")
    )
  </script>
</body>
</html>
```