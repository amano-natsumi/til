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
