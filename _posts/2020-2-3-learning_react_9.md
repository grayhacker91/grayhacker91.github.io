---
title: "리액트 이벤트"
date: 2020-2-3
categories:
  - React
tags:
  - js
  - front-end
---

## React 이벤트 다루기

이벤트를 다루기 위해서 버튼을 누를 때마다 카운트하는 간단한 예제로 알아보자.
버튼이 클릭될 때마다 값을 증가시키기 위해서는

1. 버튼의 클릭 이벤트를 리스닝하고 이벤트 핸들러를 지정해야 한다.
2. 상태 값인 카운트 값을 증가시킬 이벤트 핸들러 함수를 구현한다.

컴포넌트를 구성하면 아래와 같다. 이벤트 핸들러 함수의 이름은 increase 이다.

```javascript
class CounterParent extends React.Component {
  constructor() {
    super()
    this.state= {
      count: 0,
    }
    this.increase = this.increase.bind(this);
  },
  increase: function(e) {
    this.setState({
      count: this.state.count + 1,
    });
  },
  render() {
    var backgroundStyle = {
      //style
    };
    var buttonStyle = {
      //style
    };
    return (
      <div style={backgroundStyle}>
        <Counter display={this.state.count}/>
        <button onClick={this.increase} style={buttonStyle}>+</button>
      </div>
    );
  }
};
```

JSX 콜백에서 this 의 의미에 대해 주의해야한다. 자바스크립트에서 클래스 메서드는 기본적으로 bound 되지 않기 때문에, 만약 this.handleClick 바인드를 잊은채로 onClick 에 전달하면 this 는 함수가 실제로 호출될 때 undefined 로 취급된다.
+버튼이 클릭할 때마다 count 상태값이 1씩 증가한다.

## 합성 이벤트

리액트에서는 JSX에 이벤트를 지정하는 경우 DOM이벤트를 직접 다루지 않고, 리액트의 특정적인 이벤트 유형인 합성 이벤트(Synthetic Event)를 다룬다.
이 이벤트 핸들러는 MouseEvent나 KeyboardEvent와 같은 네이티브 이벤트를 직접받지 않고, 항상 네이티브 이벤트를 래핑한 SyntheticEvent타입을 인자로 받는다.
SyntheticEvent는 DOM이벤트와 일대일로 대응하지 않기 때문에 합성 이벤트와 그 속성을 사용할 때 DOM 이벤트 문서를 참고하지 말고 SyntheticEvent문서를 참고하자.
<https://facebook.github.io/react/docs/events.html>

속성을 알았기 때문에 간단하게 shift key를 누르면 10씩 증가하게 increase함수를 수정하자.

```javascript
  increase: function(e) {
    var curCnt = this.state.count;
    curCnt = e.shiftKey ? curCnt+10 : curCnt+1;
    this.setState({
      count: curCnt,
    });
  },
```

## 컴포넌트에서 이벤트 처리

결론을 얘기하면 컴포넌트의 이벤트는 직접 리스닝할 수 없다. 컴포넌트는 DOM 엘리먼트를 감싸는 래퍼이기 때문에 직접 리스닝 할 수 없다.
따라서 컴포넌트에 이벤트를 리스닝하기 위해서는 아래와 같이 속성으로 전달해주어야 한다.

```javascript
class CounterParent extends React.Component {
  render() {
    ...
    return (
      <div style={backgroundStyle}>
        <Counter display={this.state.count}/>
        <PlusButton clickHandler={this.increase} />
      </div>
    );
  }
};
class PlusButton extends React.Component {
  render() {
    return(
      <button onClick={this.props.clickHandler} style={buttonStyle}>
        +
      </button>
    );
  }
}
```

## 일반적인 DOM 이벤트 리스닝

SyntheticEvent에 모든 DOM 이벤트가 대응되지 않기 때문에 사용자기 지정한 이벤트는 위의 방식으로 리스닝 할 수 없다.
따라서 이 경우에는 컴포넌트 생명주기 메소드에 addEventListener를 사용해야 한다.

```javascript
class MyComponent extends React.Component {
  handleMyEvent(e) {
    //  logic
  }
  componentDidMount() {
    window.addEventListener("someEvent", this.handleMyEvent);
  }
  componentWillUnmount() {
    window.removeEventListener("someEvent", this.handleMyEvent);
  }
}
```

컴포넌트가 렌더링되면 리스닝을 시작하고 컴포넌트가 제거되면 이벤트를 리스닝 목록에서 제거해야 한다.
그렇지 않으면 컴포넌트가 제거되더라도 이벤트가 남아 있어서 오동작 할 수 있다.

### 전체 예제 코드

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
      padding: 50px;
      background-color: #FFF;
    }
  </style>
</head>
<body>
  <div id="container"></div>
  <script type="text/babel">
    class Counter extends React.Component {
      render() {
        var textStyle = {
          fontSize:72,
          fontFamily:"sans-serif",
          color:"#333",
          fontWeight:"bold",
        };
        return (
          <div style={textStyle}>
            {this.props.display}
          </div>
        );
      }
    };
    class PlusButton extends React.Component {
      render() {
        var buttonStyle = {
          fontSize:"1em",
          width:30,
          height:30,
          fontFamily:"sans-serif",
          color:"#333",
          fontWeight:"bold",
          lineHeight:"3px",
        };
        return(
          <button onClick={(e) => this.props.clickHandler(e)} style={buttonStyle}>
            +
          </button>
        );
      }
    }
    class CounterParent extends React.Component {
      constructor() {
        super();
        this.state = {
          count: 0,
        };
        this.increase = this.increase.bind(this);
      }
      increase(e) {
        var curCnt = this.state.count;
        curCnt = e.shiftKey ? curCnt+10 : curCnt+1;
        this.setState({
          count: curCnt,
        });
      }
      render() {
        var backgroundStyle = {
          padding:50,
          backgroundColor:"#FFC53A",
          width:250,
          height:100,
          borderRadius:10,
          textAlign:"center",
        };
        return (
          <div style={backgroundStyle}>
            <Counter display={this.state.count}/>
            <PlusButton clickHandler={this.increase}/>
          </div>
        );
      }
    };
    ReactDOM.render(
      <div>
        <CounterParent/>
      </div>,
      document.querySelector("#container")
    );
  </script>
</body>
</html>
```
