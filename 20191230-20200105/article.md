## Next.js
日本語ドキュメント  
https://nextjs-docs-ja.netlify.com/docs  

使い方  
```
yarn add next react react-dom
```

- (webpack と babel を利用した) 自動的なトランスパイルとビルド
- コードのホットリローディング
- ./pages 配下のファイルの構造化とサーバーサイドレンダリング
- 静的ファイルの配布./public/ は / にマッピングされます (プロジェクトに ./static/ ディレクトリを作成する必要があります。)

### cssに関して
https://qiita.com/tetsutaroendo/items/8e3351bc4bfbb419f662  
Next.js的にはstyled-jsxが推奨？  
ちょっと嫌だな… なんかキモい  

### dynamic routing
[hoge]で命名したファイルorディレクトリを作成すると反応する。  
```
/hoge/:fuga
hoge [fuga]/index.js
or
hoge/[fuga].js
```

### Nextのチュートリアルで出てきたfetchのポリフィル？
isomorphic-unfetch  
https://github.com/developit/unfetch/tree/master/packages/isomorphic-unfetch  
クライアント・サーバーどちらでも使えるfetchっぽい。  

### getInitialProps
Next.jsに用意された機能で、propsの初期値をここで入れられる。  

## Reactノート
### hookとは
https://ja.reactjs.org/docs/hooks-intro.html

ファンクショナルコンポーネントでstateを使えるようにするもの。
```
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}

// こう書いていたものが、↓のようにかける

import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

### useEffectとは
componentDidMount と componentDidUpdate と componentWillUnmount がまとまったもの。

### Hooks APIの良記事

https://programmagick.com/blogs/react_hooks_api

### React Routerのform(submit)対応
https://gist.github.com/elitan/5e4cab413dc201e0598ee05287ee4338  

## redux
### Reduxの設計方針

- Domain data: アプリケーションが表示したり変更したりするデータ
  - e.g. TODOアプリなら、todo/doing/done
- App state: アプリケーション独自の振る舞いのためのデータ
  - e.g. データの選択状態やデータフェッチのローディング状態
- UI state: 現在の表示方法のためのデータ
  - e.g. モーダルが開かれているかどうか

```
{
    domainData1 : {},
    domainData2 : {},
    appState1 : {},
    appState2 : {},
    ui : {
        uiState1 : {},
        uiState2 : {},
    }
}
```

## Docker
### Docker入門系記事
読んだ。  
https://www.enisias.cloud/docker/33/  
