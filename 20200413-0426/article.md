## React
### CSSModule
#### scssのimportについて
変数やmixinはimportを行ったscssをcssmoduleとして読み込んだときに扱える。  
また、他のscssで定義したクラスをimportすると、それも扱える。  
```
// elements.scss
.button {}

// componentA.scss
import 'elements.scss';

// componentA.jsx
import css from 'componentA.scss';

<button className={css['button']}></button>
```

## Angular
angular-after-tutorialという公式チュートリアルを終えたあとにオススメのチュートリアルをやった。  
まだ内容はちゃんと理解できていなく、写経したような感じ  
https://gitbook.lacolaco.net/angular-after-tutorial/  

## その他日記
コロナウイルス(COVID-19)という感染症で世界中が大変なことになっている。  
会社もリモートワークに以降していて2ヶ月ぐらい出社していない気がする。  
