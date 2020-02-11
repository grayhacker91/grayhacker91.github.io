---
title: "리액트 라이프사이클"
date: 2020-2-11
categories:
  - React
tags:
  - js
  - front-end
---

## React 라이프 사이클 메서드

모든 리액트 컴포넌트에는 라이프 사이클이 존재한다. 컴포넌트의 라이프 사이클은 컴포넌트가 렌더링되기 전 준비과정부터 시작해 컴포넌트가 제거될 때 끝난다.
컴포넌트를 다루다보면 최초 렌더링을 할 때 특정 작업을 한다던지 또는, 컴포넌트 상태가 변경 전 후로 특정 작업을 수행할 경우가 있다.
이때 사용하는 것이 라이프 사이클 메서드다.

라이프 사이클은 크게 3가지로 구분되며, 생성(마운트), 갱신(업데이트), 제거(언마운트)로 나뉜다.

![리액트 라이프 사이클](/images/react_lifecycle.JPG)

## 생성

DOM이 생성되고 웹브라우저에 나타나는 것을 마운트라고 합니다. 이때 호출되는 순서는 다음과 같다.

constructor -> getDerivedStateFromProps -> render -> componentDidMount

- constructor : 컴포넌트 생성자 메서드, 초기 state값을 지정할 수 있음.
  
  ```javascript
  constructor(props) {
    super(props);
    this.state = { counter: 0 };
    this.handleClick = this.handleClick.bind(this);
  }
  ```

- static getDerivedStateFromProps : props에 있는 값을 state에 동기화하는 작업을 할 때 사용하는 메서드, 컴포넌트를 마운트하거나, props를 변경할 때 호출됨.
  
  ```javascript
  static getDerivedStateFromProps(props, state) {
    if(props.value != state.value) {
      return { value : props.value }
    }
    return null;
  }
  ```

- render : UI 렌더링 메서드
- componentDidMount : 컴포넌트가 웹브라우저 상에 렌더링 된 후에 호출되는 메서드
  
  ```javascript
  componentDidMount()
  ```

## 업데이트

컴포넌트가 업데이트 될 때는 아래 4가지 경우이다.

1. props가 변경될 때
2. state가 변경될 때
3. 부모 컴포넌트가 리렌더링 될 때
4. this.forceUpdate로 강제로 렌덜이을 트리거 할 때

순서는 다음과 같다.

getDerivedStateFromProps -> shouldComponentUpdate -> render -> getSnapshotBeforeUpdate -> componentDidUpdate

- getDerivedStateFromProps : props에 있는 값을 state에 동기화하는 작업을 할 때 사용하는 메서드, 컴포넌트를 마운트하거나, props를 변경할 때 호출됨.
- shouldComponentUpdate : 컴포넌트가 리렌더링 할지 말지를 결정하는 메서드
  
  ```javascript
  shouldComponentUpdate(nextProps, nextState) {
    // true를 리턴하면 리렌더링하고 false를 리턴하면 리렌더링하지 않는다.
    return true;
  }
  ```

- render : 컴포넌트를 리렌더링 한다.
- getSnapshotBeforeUpdate : 컴포넌트 변화를 DOM에 반영하기 바로 직전에 호출하는 메서드

  ```javascript
  getSnapshotBeforeUpdate(prevProps, prevState) {
    // 이 메서드가 반환하는 값은 componentDidUpdate에 인자로 전달된다.
    return null;
  }
  ```

- componentDidUpdate : 컴포넌트의 업데이트 작업이 끝난 후 호출하는 메서드

  ```javascript
  componentDidUpdate(prevProps, prevState, snapshot)
  ```

## 제거

컴포넌틀를 DOM에서 제거하는 것을 언마운트라고 한다. 이때 호출되는 메서드는 하나뿐이다.

- componentWillUnmount : 컴포넌트가 DOM에서 제거되기 전에 호출하는 메서드
  
  ```javascript
  componentWillUnmount() {
    // 이벤트, 타이머, 네트워크 요청 등 모든 정리 작업을 수행해야 한다.
  }
  ```

## 에러

라이프 사이클에는 포함되지 않지만, 리액트에서 render함수에서 에러가 발생할 시, 사용하는 API가 있다.
componentDidCatch로 자신의 render함수에서 발생하는 에러는 잡아낼 수 없지만. 하위 컴포넌트에서 발생하는 에러는 잡아낼 수 있다.
실제로 아래와 같이 작성해서 사용할 수 있다.
(참고 : <https://ko.reactjs.org/docs/error-boundaries.html>)

```javascript
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
  static getDerivedStateFromError(error) {
    // Update state so the next render will show the fallback UI.
    return { hasError: true };
  }
  componentDidCatch(error, errorInfo) {
    // You can also log the error to an error reporting service
    logErrorToMyService(error, errorInfo);
  }
  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}
```
