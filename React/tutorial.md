### React要素

```javascript
const element = <div>hello</div>;
```

### 関数コンポーネント
- propsを引数にとりReact要素を返す  
- 大文字から始める  

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

### クラスコンポーネント

```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

## components
- コンポーネントが使用されるコンテキストではなく、コンポーネント自身からの観点で props の名前を付けることをお勧めします。
- 全ての React コンポーネントは、自己の props に対して純関数のように振る舞わねばなりません。


## state
- this.state は特別な意味
- 何かデータフローに影響しないデータ（タイマー ID のようなもの）を保存したい場合に、追加のフィールドを手動でクラスに追加可能

```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

### setState
- 関数を引数に取る事ができる

```javascript
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```

## イベント
### thisをbindする方法

```javascript
  constructor(props) {
    super(props);
    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }
```

または

```
  handleClick = () => {
    console.log('this is:', this);
  }
```

または

```
  render() {
    // This syntax ensures `this` is bound within handleClick
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
```

### 引数を渡す

```
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
  OR
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

## 条件
- && 
false は無視される

```
return (
  <div>
    {unreadMessages.length > 0 && <div>hello</div>}
  </div>
);
```

− render でnullを返すと描写されない

## 静的なバージョン
- 静的なバージョンを作るときはstateを使わない

## フラグメント
コンポーネントが複数の要素を返す場合 フラグメントでまとめる
- 複数の要素をグループ化

```
import React, { Fragment } from 'react';

function ListItem({ item }) {
  return (
    <Fragment>
      <dt>{item.term}</dt>
      <dd>{item.description}</dd>
    </Fragment>
  );
}

function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        <ListItem item={item} key={item.id} />
      ))}
    </dl>
  );
}
```

省略記法

```
function ListItem({ item }) {
  return (
    <>
      <dt>{item.term}</dt>
      <dd>{item.description}</dd>
    </>
  );
}
```

## コード分割
- 1つのファイルにまとまる
- exportを書く

```
// app.js
import { add } from './math.js';

console.log(add(16, 26)); // 42
```

```
// math.js
export function add(a, b) {
  return a + b;
}
```

### 遅延import
- default にする
- Suspenseで読み込むまでのローディング表示してくれる

```
// todo.js
export default Todo;

// index.js
const Todo = React.lazy(() => import('./todo'));
ReactDOM.render(
  <React.Suspense fallback={<div>Loading...</div>}>
    <Todo />
  </React.Suspense>,
  document.getElementById('root')
);
```

## コンテクスト

```
const TodoTitle = React.createContext("");

// 親
    return (
      <TodoTitle.Provider value={this.props.title}>
        <div>
          <ul>
            {listItems}
          </ul>
          <ItemInput onClick={this.addItem} />
        </div>
      </TodoTitle.Provider>
    );

// 子
class TodoItem extends React.Component {
  static contextType = TodoTitle;
}
// 値を参照するとき
this.context

// 関数コンポーネントの場合
<TodoTitle.Consumer>
  {title => (…)}
</TodoTitle.Consumer>
```
