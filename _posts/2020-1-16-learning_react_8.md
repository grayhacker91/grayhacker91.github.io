---
title: "리액트 상태 다루기"
date: 2020-1-16
categories:
  - React
tags:
  - js
  - front-end
---

## React의 상태 다루기

웹앱은 상호적인 시나리오를 많이 가지고 있고, 사용자와 상호작용 결과 또는 서버로부터 데이터로 컴포넌트의 특정 부분이 변경될 수 있다.
이때 변경될 수 있는 데이터를 상태라고 한다. 간단한 예제를 이용해서 상태 처리를 해보자.

예제는 1초마다 화면의 숫자가 변경되는 간단한 예제이고, 여기서 상태는 변경되는 숫자이다.
1초마다 값을 변경시키기 위해서는 setInterval함수를 이용할 수 있다.
그리고 리액트 컴포넌트가 제공하는 API를 사용하여 변경할 수 있다.

- componentDidMount
   이 메소드는 컴포넌트가 렌더링된 후에 실행된다.
- componentWillUnmount
   라이프 사이클 훅에서 동작한다.
- setState
   state 객체의 값을 갱실할 수 있다.

아래와 같이 state를 지정하고 갱신할 수 있습니다.

```javascript
class LightningCounter extends React.Component {
  constructor(props) {
    super(props);
    this.state= {
      strikes: 0
    }
  }
  timerTick() {
    this.setState({
      strikes:this.state.strikes+100
    });
  }
  componentDidMount() {
    this.timerID = setInterval(
      () => this.timerTick(), 1000
    );
  }
  render() {
    return (
      <h1>
        {this.state.strikes}
      </h1>
    );
  }
};
```

setState는 여러 형태로 사용할 수 있는데, 병합시키기 원하는 모든 속성을 넣을 수 있다.
setState를 통해 state 객체에 어떤 내용이 변경될 때마다 컴포넌트의 render메소드가 자동으로 호출되서 갱신된다.
