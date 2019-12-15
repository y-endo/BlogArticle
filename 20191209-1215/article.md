## Vue + TypeScript
以前Vueで作成した家計簿WebアプリをTypeScript化した。  
https://github.com/y-endo/kakeiboApp/tree/feature/v0.0.0-typescript  
全体的に製作コストが高くなる。  
デコレータ（vue-property-decorator）の癖が強くて苦戦。  
@Emitの定義でメソッドが増えてしまうのが嫌。  

## Vueで知らなかった機能
dataの変更によるDOMの変更をキャッチする  
$nextTickを使えば、dataの変更によるDOMの書き換えをキャッチできる  
```
<div v-for="item in itemData" class="hoge">

methods: {
  remake() {
    this.itemData = data;
    this.$nextTick(() => {
      console.log('.hoge changed');
    });
  }
}
```

## 独自の型定義ファイルを作成した
https://qiita.com/mtgto/items/e30d1529ca298e49557e  
以下の書き方は便利そう  
```
"compilerOptions": {
  "paths": { "*": ["types/*"] },
  "typeRoots": ["types", "node_modules/@types"]
}

import { hogeType } from 'hoge';
```

## Reactの勉強も開始
### Babel + webpackの簡単な開発環境を構築
https://qiita.com/akirakudo/items/77c3cd49e2bf39da79dd

### stateについて
Reactでstateを変更する場合は、React.Componentクラスを継承して、setStateメソッドを使う
```
class Hoge extends React.Component {
  constructor() {
    super();
    this.state = { name: 'yuki' };
  }

  render() {
    setTimeout(() => {
      // 直接stateを変更しても効かない
      // this.state.name = 'endo';

      this.setState({ name: "endo" })
    }, 1000);
    return (<div>{ this.state.name }</div>)
  }
}
```

### Propsについて
ReactでPropsを渡す時は属性={値}で渡す  
this.propsでデータにアクセスできる
```
const name = 'Yuki';

<Hoge name={name}>
```
また、コンポーネントを複数作成して、異なるpropsを渡せば、それぞれ異なるpropsを持ったコンポーネントができる  
```
<Hoge name={'Endo'}>
<Hoge name={'Yuki'}>
```

### Propsでメソッドも渡せる
メソッドをpropsで渡すことで、他のコンポーネントから渡したメソッドを実行できる  
```
fugaMethod() {
  console.log('fuga');
}
<Hoge changeText={this.fugaMethod.bind(this)}>

// Hoge
this.props.fugaMethod(); // fuga
```

### public class fields syntaxでbindの記載を省略できる
```
fugaMethod = () => {
  console.log('fuga');
}
<Hoge changeText={this.fugaMethod}>

// Hoge
this.props.fugaMethod(); // fuga
```

### Routerについて
```
import { BrowserRouter as Router, Route } from 'react-router-dom';

ReactDOM.render(
  <Router>
    <Router exact path="/" component={Top}>
  </Router>,
  app
)
```
/ にアクセスした時にTopコンポーネントが表示される。  
/hoge にアクセスした場合もTopコンポーネントが表示されてしまうので、それを防ぐためにexactを追加する。  
exactがあると厳密にパスが合っていないと表示されない。

## Jestにも触れた
がっつりはやっていないけど、なんとなくの雰囲気はつかめた  
### Jestの入門良記事
https://www.wakuwakubank.com/posts/525-javascript-jest/

### Jestインストール
```
yarn add -D jest
```

### Jestとは？
Facebook製のJavaScriptテストツール  
Reactと相性が良い。React専用という訳ではない  

### ドキュメント
https://jestjs.io/docs/ja/getting-started.html

### テストファイル
*.test.js、*.spec.js もしくは __tests__ ディレクトリ以下のファイルをテストファイルとみなす。  
testRegexオプションで変更できる。  

### test()関数
テストをする場合、test()関数を実行する。  
第一引数にテストの概要、第二引数にテスト内容を関数として記述する。  
テスト関数の中で、expect()関数を使ってテストする。  
expect()の返り値に対してマッチャーを使って値をチェックする。  
```
test('テストの概要をここに書く', () => {
  expect(hello('test')).toBe('Hello test');
})
```
test関数は、it関数でも同じ動作をする

### describe(name, fn)関数
describeは複数の関連したテストを1つの「テストスイート」としてグループ化したブロックを作成する。  
```
const myBeverage = {
  delicious: true,
  sour: false,
};

describe('my beverage', () => {
  test('is delicious', () => {
    expect(myBeverage.delicious).toBeTruthy();
  });

  test('is not sour', () => {
    expect(myBeverage.sour).toBeFalsy();
  });
});
```

### マッチャ―の種類
https://jestjs.io/docs/ja/expect.html#content  

よく使うマッチャ―  
```
// 等しい
expect().toBe()

// not（〜でない）
expect().not.toBe()

// toBeは厳密な等価性を判定するので、オブジェクトの場合はtoEqualを使う
expect({ a: 100 }).toEqual({ a: 100 })

// 大きい
expect(4).toBeGreaterThan(3)
expect(4).toBeLessThanOrEqual(4)

// null, true, false
expect(null).toBeNull()
expect(true).toBeTruthy()
expect(false).toByFalsy()

// 文字列
expect('hoge').toMatch(/ge/)
expect('hoge').not.toMatch(/ao/)

// 配列
expect([1, 2, 3]).toContain(2)

// 例外
const fuga = () => {
  throw new Error('error message fuga');
}

expect(fuga).toThrow('error message xxx');
```

### jest-dom
DOMの状態をテストするカスタムJestマッチャ―  
https://github.com/testing-library/jest-dom

### Vueの公式テストライブラリ
vue-test-utils  
https://vue-test-utils.vuejs.org/ja/

### Vue + Jestのテスト
この辺のモジュールがテストに必要  
```
"devDependencies": {
  "@vue/test-utils": "^1.0.0-beta.30",
  "babel-jest": "^24.9.0",
  "jest": "^24.9.0",
}
```

test utilsのmount()メソッドを使って、コンポーネントのラッパを作成する。
```
import { mount } from '@vue/test-utils';
import ComponentHoge from './ComponentHoge';

// コンポーネントがマウントされ、ラッパが作成される
const wrapper = mount(ComponentHoge);

// wrapper.vmを介して、実際のVueインスタンスにアクセスできる
const vm = wrapper.vm
```

### ユーザーのインタラクションのシミュレート
wrapper.find('selector')で要素を取得できる。  
要素のメソッド「.trigger('eventName')」でシミュレートできる  
```
const button = wrapper.find('button');
button.trigger('click');
```