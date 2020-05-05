## Angular
### Angular After Tutorial
Angularチュートリアルを終えた人向けのコンテンツ。  
https://gitbook.lacolaco.net/angular-after-tutorial/  

### ChangeDetectionStrategy.OnPush
Angularのパフォーマンス改善で指定するもの。  
https://qiita.com/masaks/items/61150907ce95b509fcaa  
OnPushに指定されたComponentはアタッチ時に初回のチェックを行い、あとは外部の変更によるトリガーでチェックされることがない。  

### AsyncPipe
Asyncパイプでオブジェクトの子要素を出力したい時  
```
<div>{{ (user$ | async)?.name }}</div>
```

基本的な方針としてAyncパイプで解決できるケースはAsyncパイプを利用し、subscribe等の明示的な購読は最低限に留める。明示的な購読をした場合購読解除について考える必要がある。  
Asyncパイプを使えば、コンポーネントのライフサイクルに合わせて自動的に購読・購読解除がおこなわれる。  

### ng-container
https://qiita.com/shibukawa/items/c8c7fd22c1054348db3a  
ReactのFragment的なもの。  
```
<ng-container>
  <p>A</p>
</ng-container>
↓
<p>A</p>
```

### ng-template
表示されない。templateタグ的なもの。  
#で名前をつけて、ローディング中に出す別の要素として使える。  
```
<div *ngIf="loaded; else loading"></div>
<div *ngIf="loaded; then loaded; else loading"></div>
<ng-template #loaded>
	loaded
</ng-template>
<ng-template #loading>
	loading...
</ng-template>
```

### ng-content
Reactのchild的なもの。  
```
// outer.component.html
<div class="outer">
	<ng-content></ng-content>
</div>

<app-outer>
	<p>テキスト</p>
</app-outer>
↓
<div class="outer">
	<p>テキスト</p>
</div>
```

### ngIfによるテンプレートのブロック化
Asyncパイプを使う時に同じObservableに複数回Asyncパイプを適用できるが、同じObservableを2度購読することになるので非推奨。  
### ngIf-as
ngIf-as構文を使うとスマートに実装できる。  
```
{{ user$ | async }}
{{ user$ | async }}
<ng-container *ngIf="user$ | async as user">
	{{ user.name }}
	{{ user.name }}
</ng-container>
```
user$がnullの時*ngIfの評価がfalseになるので内側のブロック全体が表示されなくなる。  
*ngIfのelse構文で対処できる。  
```
<ng-container *ngIf="user$ | async as user; else userIsNull">
	{{ user.name }}
</ng-container>
<ng-template #userIsNull>
	User is Null
</ng-template>
```
### as-snapshotパターン
else構文でカバーできるがテンプレートが肥大化しがち。  
そもそもの原因がObservableがnullを流した時に*ngIfがfalseと評価してしまうから。  
なので常にtrueになるような評価式にしておくと良い。  
```
<ng-container *ngIf="{ user: user$ | async } as snapshot">
	{{ snapshot.user?.name }}
</ng-container>
```
as-snapshotパターンの注意点  
**複数の無関係なAsyncパイプを内包させてはいけない。**  
```
<ng-container *ngIf="{ foo: foo$ | async, bar: bar$ | async } as snapshot">
  <div>Foo: {{ snapshot.foo }}</div>
  <div>Bar: {{ snapshot.bar }}</div>
</ng-container>
```
この場合foo$かbar$のどちらかが更新されると内部全体が再評価されてしまう。  
このような場合は冗長になるがちゃんと分けるべき。  
```
<ng-container *ngIf="{ foo: foo$ | async } as snapshot">
  <div>Foo: {{ snapshot.foo }}</div>
</ng-container>
<ng-container *ngIf="{ bar: bar$ | async } as snapshot">
  <div>Bar: {{ snapshot.bar }}</div>
</ng-container>
```

### Form
https://angular.jp/guide/forms-overview  
https://angular.jp/guide/reactive-forms  
主にリアクティブフォームを使いことになりそう？  
FormBuilderなどを使うには、AppModuleにFormsModuleとReactiveFormsModuleをimportする。  
#### [formGroup]とformGroupNameの違い。 [formControl]とformControlName の違い。
formGroupNameはネスト用かな？[formGroup]の中に入れ子のグループがある場合、そいつはformGroupName指定にする。  

formControlも考え方は同じだけど、逆。  
[formControl]が入れ子ようで、formControlNameは[formGroup]の子。  
https://stackoverflow.com/questions/40171914/what-is-the-difference-between-formcontrolname-and-formcontrol  

## RxJS
### Subject
Subjectとその種類  
https://qiita.com/bouzuya/items/1d3251e2c5a53b856d1a  

### 2種類のObservable
Observableには2種類ある。  
購読者の有無に関わらなずストリームが流れつづけるHot Observable、購読者がいるときだけストリームを流すCold Observable。  