## React
### フラグメント
React でよくあるパターンの 1 つに、コンポーネントが複数の要素を返すというものがあります。フラグメント (fragment) を使うことで、DOM に余分なノードを追加することなく子要素をまとめることができるようになります。
```
render() {
  return (
    <React.Fragment>
      <ChildA />
      <ChildB />
      <ChildC />
    </React.Fragment>
  );
}

// 短い記法
render() {
  return (
    <>
      <ChildA />
      <ChildB />
      <ChildC />
    </>
  );
}
```

### constructorでsuper(props)をやる理由
https://qiita.com/hand-dot/items/61a4b808f110b12e4281  
super()だけでもthis.propsが使えるけど、super(props)って書いたほうが良さそう

### ライフサイクルの詳しい解説
qiitaの良記事  
https://qiita.com/Julia0709/items/3c3fc8d29fd2e56ed7a9

### フェッチやタイマーをセットするタイミング
componentDidMount()で行う。  
単純に静的なデータを書き換えを行うと、ムダに2回レンダリングされる。  
直接のDOM操作は原則避ける

### アニメーションやタイマーを削除するタイミング
componentWillUnmount()で削除する。  
コンポーネントを破棄する直前に呼ばれるメソッド。

### データフローに影響しないデータについて
this.props は React 自体によって設定され、また this.state は特別な意味を持っていますが、何かデータフローに影響しないデータ（タイマー ID のようなもの）を保存したい場合に、追加のフィールドを手動でクラスに追加することは自由です。

### コンポーネントのrenderで何も表示させたくない場合
nullを返す。  
nullを返してもコンポーネントのライフサイクルに影響しない。  

### ループのkeyについて
keyにindexを設定するのは最終手段であり、とくに並び替えが発生する場合はユニークなidを用意するべき。  
keyにindexを設定した場合の悪影響について  
https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318

### フォームの本格的なソリューション
https://jaredpalmer.com/formik  
入力値のバリデーション、訪問済みフィールドの追跡やフォーム送信を含む完全なソリューションをお探しの場合は、Formik が人気のある選択肢のひとつです。

### コンポーネントの子要素
props.childrenで取得できる。  
```
<HogeComponent>
  <p>hogehoge</p>
</HogeComponent>

// Hoge
this.props.children
// ↓
<p>hogehoge</p>
```

### propsにコンポーネントを直接渡すこともできる
```
<Hoge child={<Fuga />} />
```

### Router v4でルーティング先にpropsを渡す方法
http://ngzm.hateblo.jp/entry/2017/06/23/001352  
```
<Router>
  <Route path="/" render={() => <Hoge hoge={'fuga'} />}>
</Router>
```

### DOMに直接アクセスする方法
Vueのref的なのがReactにもある。  
https://the2g.com/2796  
```
// constructorの中で定義
this.input = React.createRef();

<input type="text" ref={this.input}>
```

## flux
### FSAとは
https://github.com/redux-utilities/flux-standard-action
flux-standard-actionの略。  
Actionオブジェクトの構造は自由に作れるため、ルールを決めよう。  
必須条件
プレーンなJSオブジェクトであり、typeプロパティをもつこと。
type, error, payload, meta以外を含まないこと。
オプションの条件
error、payload、metaプロパティをもつこと。
payloadにデータをもたせる、形式はなんでもOK

## React + redux
## 読んだ
https://qiita.com/mpyw/items/a816c6380219b1d5a3bf
https://qiita.com/enshi/items/19b1924b72f8c2ffd1eb
https://qiita.com/suzukenz/items/40afe717029c2f8f4a54
https://qiita.com/morrr/items/2e284ae691af513edacc
https://qiita.com/cortyuming/items/bd82886ae2ec381e6edd
https://qiita.com/pullphone/items/fdb0f36d8b4e5c0ae893
https://www.wakuwakubank.com/posts/704-react-redux-connect/
https://qiita.com/tomi_linka/items/edb769bdfc80bc6c0f28

## Actionの命名規則
https://qiita.com/NewBieChan/items/cdde1e9c679ece4e6cfd
ReduxのAction名は、「システムが行うこと」ではなく「外の世界で実際に起こったこと」をベースに命名します。  

> The only way to change the state tree is to emit an action, an object describing what happened. (Redux公式より)
> Every action only describes what happened in the outside world (user wants to do this, server said that) (reduxコミッターのdtinthさんより)

この議論に乗っ取ると、例えばSNSなどのサービスで、あるユーザーがコメントを投稿する場合のAction名は、以下のようになります。
```
(x) CREATE_COMMENT
(o) POST_COMMENT
```

## mapStateToPropsとmapDispatchToProps
https://qiita.com/suzukenz/items/40afe717029c2f8f4a54

## combineReducersをについて
```
export default combineReducers({
  top: topReducer
});

Store.state.top = topReducer?
```

アプリケーションが大きくなるとStoreを構成するオブジェクトの構造が複雑になりますが、Reducerを複数の関数に分割して、更新するstateだけを処理することができます。  
例えば、userに関する処理を行うReducerをuserReducer、foodに関する処理を行うReducerをfoodReducerと定義した場合、  
```
const store = createStore(
  combineReducers({
    user: userReducer,
    food: foodReducer
  })
);
```
とすると、userReducerのstateにはstore.user以下のstate、foodReducerのstateにはstore.food以下のstateを受け取ることができます。
