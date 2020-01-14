---
title: "JSX"
date: 2020-1-14
categories:
  - React
tags:
  - js
  - front-end
---

## JSX의 동작

React에서 사용하는 JSX는 실제로 브라우저가 해석할 수 없는 내용이다.
그렇기 때문에 이 JSX를 브라우저가 알아볼 수 있는 자바스크립트로 변환해주는 바벨과 같은 툴이 필요한 이유이다.
이전에 작성했던 JSX구문으로 변환되는 내용을 살펴본다.

```javascript
class Card extends React.Component {
  render() {
    var cardStyle = {
      height: 200,
      width: 150,
      padding: 0,
      backgroundColor: "#ffffff",
      WebkitFilter: "drop-shadow(0px 0px 5px #666)",
      filter: "drop-shadow(0px 0px 5px #666)"
    };
    return (
      // 이 부분이 JSX
      <div style={cardStyle}>
          <Square color={this.props.color}/>
          <Label color={this.props.color}/>
      </div>
    );
  }
};
```

위에 return 내용을 실제로 변환될 경우 아래와 같이 React구문으로 변경된다.

```javascript
return React.createElement(
  "div",
  {stype: cardStyle},
  React.createElement(Square, {color:this.props.color}),
  React.createElement(Label, {color:this.props.color})
);
```

## JSX의 특징

1. 단 하나의 루트노드만 리턴 가능
   - JSx에서는 둘 이상의 루트 엘리먼트를 리턴하거나 렌더링할 수 없다.
   - 그 이유는 위 JSX가 React로 변경된 결과로 알 수 있다.
   - 이 요구사항은 createElement때문인데, render와 return함수에서 최종적으로 리턴하는 것은 하나의 createElement 호출이다.
   - 하나의 createElement호출로 루트노드는 하나이지만, 그 밑에 여러 개의 호출이 있어도 문제는 없다.
2. 인라인 CSS불가
   - JSX에서는 style속성안에 CSS를 포함할 수 없으며, 그 대신 스타일 정보를 담은 객체를 참조해야 한다.
3. 예약어와 className
   - 자바스크립트에서는 지정된 예약어(break, case, class 등)가 있기 때문에 JSX를 작성할 때에는 이 키워드를 사용하지 않도록 주의해야 한다.
   - 실제로 이 내용을 무시하고 사용할 경우 동작하지 않는다.
   - 만약 class속성을 지정할려고 할 때에는 DOM API 버전인 className을 사용해야 한다.
   - 리액트가 지원하는 태그와 속성은 <https://reactjs.org/docs/dom-elements.html> 링크에서 확인이 가능하다.
4. 주석
   - HTML, javascript에서와 같이 JSX에서도 비슷하게 주석이 사용가능한데 주의할 점이 있다.
   - 태그의 자식 위치에 주석을 넣을 경우 주석이 표현식으로 해석이 될 수 있도록 중괄호로 감싸아야 한다.
   - 하지만 태그안에 주석이 있는 경우에는 중괄호를 감싸지 않아도 된다.
5. 대소문자 구별
   - HTMLl엘리먼트를 나타낼 때에는 소문자로 작성해야하며, 컴포넌트를 작성할 때에는 그 이름에 대문자를 작성해야 한다.
   - 대소문자를 정확하게 구분하지 않으면, 컴포넌트를 찾지 못해 렌더링할 수 없다.
6. 어디든 가능한 JSX
   - JSX는 render와 return함수 안에서만 사용 가능한 것은 아니다.
   - 따로 변수로 선언해서 사용할 수 있고, 이 경우 변수가 실제로 사용될 때 컴포넌트가 초기화 된다.
