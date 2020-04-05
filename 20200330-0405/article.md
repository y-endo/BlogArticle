## Angular
### Routingの使い方
RouterModuleの forRoot はルートでした使えない。(app-routing.module.ts)  
他のルーティングで使う場合は、RouterModule.forChild を使う。  

### AsyncPipe
*ngForでObservableの反復処理を行う場合は、リスト変数名の末尾に$をつける。  
Observableをそのままforofでは回せないので、パイプ演算子でasyncを使用する。  
```
<li *ngFor="let item of items$ | async"></li>
```

### グローバルファイルの読み込み
cssやscriptをアプリケーション全体に反映させる(グローバル読み込み)には、angular.jsonを使うと簡単にできる。  
projects->projectName->architect->build,test
のstyleやscriptsの配列にangular.jsonからの相対パスでファイルパスを記述すると読み込まれる。  
上から順番に読み込まれていく。  

### CommonModule、BrowserModule
ngIfやngForなどの汎用的な機能を使うためにCommonModuleをimportsする必要がある。  
BrowserModuleにはCommonModuleが含まれており、app.module.tsでのみ使えば良い。  
子ModuleではCommonModuleを使う。  

### プロキシでHttpClientのCORSを回避(開発)
例えば、angularアプリはport: 4200で動いていて、APIサーバが:3000で動いていた場合、CORSになってしまい通信できない。  
それを回避する方法を以下の記事で紹介している。  
npm scriptsでやる方法を紹介しているけど、angular.jsonに書くほうがスマートっぽい。  
https://qiita.com/ksh-fthr/items/a462a96de7080092b73c  
https://angular.jp/guide/build  

### ディレクトリ構成
参考にした記事。  
https://itnext.io/choosing-a-highly-scalable-folder-structure-in-angular-d987de65ec7  

### サービスをpublicとprivateどちらで使うべきか
privateで宣言した場合、テンプレートファイル内で参照できない。  
なので、privateで宣言して、クラスのメソッドからサービスのメソッドを実行する形にするのが良いらしい。  
https://stackoverflow.com/questions/46596399/typescript-dependency-injection-public-vs-private

## RxJS
### 入門記事
https://qiita.com/katsunory/items/75651919f6786864c2b5  
https://qiita.com/katsunory/items/6635a243abd7c5fb74ea  


### 基本概念
- Observable: イベントや値をRxJSで受け取れる形にする
- Operators: 受け取ったイベントや値を加工する
- Subject: Observableを同時にいろんなところで受け取れるようにする
- Subscription: subscribeの解除を行う  

### subscribeメソッドの引数
本来はnext, error, completeの連想配列で引数を渡す形になっているが、無名関数を順番に渡すだけでも大丈夫。  
```
observable.subscribe({
  next: x => console.log('got value ' + x),
  error: err => console.error('something wrong occurred: ' + err),
  complete: () => console.log('done')
});
// ↓
observable.subscribe(
  x => console.log('got value ' + x),
  err => console.error('something wrong occurred: ' + err),
  () => console.log('done')
);
```