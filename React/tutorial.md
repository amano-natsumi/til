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
