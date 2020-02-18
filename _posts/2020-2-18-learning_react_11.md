---
title: "Todo List 제작"
date: 2020-2-18
categories:
  - React
tags:
  - js
  - front-end
---

간단한 Todo List 제작

input태그에 값을 넣어 버튼을 누르면 addItem이 호출되서 상태 값이 변경되고, 변경된 상태 값에 따라 TodoItem값이 추가된다.

```html
<!DOCTYPE html>
<html>
<head>
  <title>React! React! React!</title>
  <script src="https://unpkg.com/react@16.12.0/umd/react.development.js"></script>
  <script src="https://unpkg.com/react-dom@16.12.0/umd/react-dom.development.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.23/browser.min.js"></script>
</head>
<style>
  body {
    padding: 50px;
    background-color: #66ccFF;
    font-family: sans-serif;
  }
  .todoListMain .header input{
    padding: 10px;
    font-size: 16px;
    border: 2px solid #FFF;
  }
  .todoListMain .header button{
    padding: 10px;
    font-size: 16px;
    margin: 10px;
    background-color: #0066FF;
    color: #FFF;
    border: 2px solid #0066ff;
  }
  .todoListMain .header button:hover{
    background-color: #003399;
    border: 2px solid #003399;
    cursor: pointer;
  }
  .todoListMain .theList {
    list-style: none;
    padding-left: 0;
    width: 255px;
  }
  .todoListMain .theList li {
    color: #333;
    background-color: rgba(255, 255, 255, .5);
    padding: 15px;
    margin-bottom: 15px;
    border-radius: 5px;
  }
</style>
<body>
  <div id="container"></div>
  <script type="text/babel">
    class TodoItems extends React.Component {
      render() {
        var todoEntries = this.props.entries;
        var createTasks = function(item) {
          return <li key={item.key}>{item.text}</li>
        }
        var listItems = todoEntries.map(createTasks);
        return(
          <ul className="theList">
            {listItems}
          </ul>
        );
      }
    }
    class TodoList extends React.Component {
      constructor() {
        super();
        this.state = {
          items: [],
        }
        this.addItem = this.addItem.bind(this);
      }
      addItem(e) {
        var itemArray = this.state.items;
        itemArray.push(
          {
            text: this._inputElement.value,
            key: Date.now()
          }
        );
        this.setState({
          items: itemArray,
        });
        this._inputElement.value = "";
        e.preventDefault();
      }
      render() {
        return (
          <div className="todoListMain">
            <div className="header">
              <form onSubmit={this.addItem}>
                <input ref={(a)=> this._inputElement=a} placeholder="Enter Task"></input>
                <button type="submit">add</button>
              </form>
            </div>
            <TodoItems entries={this.state.items}/>
          </div>
        );
      }
    }

    var destination = document.querySelector("#container");
    ReactDOM.render(
      <div>
        <TodoList/>
      </div>,
      destination
    );
  </script>
</body>
</html>
```
