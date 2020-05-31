## Angular
プロジェクト作成時、テストファイルの生成をスキップしたい場合  
```
ng new ProjectName --skip-tests
```
e2eフォルダーやtest.tsファイルは自分で削除する必要がある。  

### 双方向データバインディング
[(ngModel)] が双方向データバインディング構文になる。  
```
<input [(ngModel)]="name" />
```
テンプレート駆動フォームで双方向データバインディングを行う場合、formを含める事ができない。  
inputタグをformタグの中に入れた状態でngModelを指定してもエラーがでる。  

---

## Akita
状態管理ライブラリ。  
NgRXより簡単で使いやすい。  
https://netbasal.gitbook.io/akita/  
https://qiita.com/m-hamada/items/89ee8268f5205cf77e19  

### セットアップ
npm、yarnでインストールする方法もあるが、Angularのcliで追加した方がコードの補完もしてくれて楽。  
```
ng add @datorama/akita
```

---

## React
### Redux devtools
Chromeの拡張機能。発生したアクションの履歴とかを見えるようにするらしい。  
https://harkerhack.com/react-redux-devtools/  

---

## CSS
### CSSカスタムプロパティ(CSS変数)
CSSで使える変数。IE以外のモダンブラウザで使える。  
変数を定義するには--から始まる変数名を宣言する。  
```
--text-color: #ccc;
```
変数を呼び出すにはvar()関数を使う。  
```
p {
  color: var(--text-color);
}
```
CSSカスタムプロパティが使えない場合のフォールバック  
CSSカスタムプロパティが宣言されていなかったり無効になっている場合に代わりの値を適用させる。  
```
p {
  color: var(--text-color, #333);
}
```
基本的にはグローバルに変数を定義して使う事が多いらしい。  
グローバル変数として定義するには:rootの中で宣言する。  
```
:root {
  --text-color: #ccc;
}
```
別の要素で変数を上書きされた場合、ローカルスコープで新しく定義された値が適用される。  
```
:root {
  --text-color: #ccc;
}
p {
  color: var(--text-color);
}
.blue {
  --text-color: blue;
}

<p>テキストは#333</p>
<p class="blue">テキストはblue</p>
```
#### IE11などレガシーブラウザでも使う場合の対策
@supporsブロックでクロスブラウザ対応を行う。  
```
section {
  color: blue;
}
@supports(--css: variables) {
  section {
    --my-color: blue;
    color: var(--my-color, 'blue');
  }
}
```

---

## Deno
Node.jsの製作者が開発した新しいJS/TSランタイム。  
https://qiita.com/so99ynoodles/items/c3ba2a528052827e3b3c  
https://qiita.com/azukiazusa/items/8238c0c68ed525377883  