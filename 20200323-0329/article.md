## Angular
### インストール
angular/CLIを使うのが一般的っぽい。  
```
npm i -g @angular/cli
```
ngコマンドが使えるようになるので、プロジェクトを作る  
```
ng new ProjectName
```
プロジェクトをフォルダ内で以下のコマンドを打つとlocalhost:4200でserveできる。
```
ng serve
```

### ファイル構成
フォルダ内にHTML、CSS、TSをまとめて1つのコンポーネントになっている。  
hoge
　- hoge.component.css
　- hoge.component.html
　- hoge.component.ts


### ダブルカーリーブラケット
{{}}のこと。Vueと一緒。

### *ngFor
AngularのHTMLに対するfor処理。  
```
<p *ngFor="let item of items">{{ item }}</p>
```
#### *ngForディレクティブで利用できる特殊変数
- index: 要素のインデックス
- first: 最初の要素か
- last: 最後の要素か
- even: インデックスが偶数か
- odd: インデックスが奇数か
```
// Idの中にindexが入る。
<li *ngFor="let item of items; index as Id"></li>
<li *ngFor="let item of items; let Id = index;"></li>
```

### *ngIf
AngularのHTMLに対するif処理
```
<p *ngIf="isEnable">Enable</p>
```

### プロパティバインディング
[プロパティ]="value"  
プロパティバインディングがバインドの対象とするのはプロパティであって、属性ではない。  
なので、属性としては存在するけど、プロパティとして存在しないものは正しく動作しない。  
カスタムデータ属性などはできないということ。
```
<a href="#" [title]="data.title">アンカー</a>
```

### 属性バインディング
属性バインディングは、属性にバインドできる。  
カスタムデータ属性なんかはこちらを使う。  
使い方はプロパティバインディングの接頭語に「attr.」をつける。  
```
<a [attr.data-href]="hoge">アンカー</a>
```

### イベントバインディング
(eventName)="method"  
HTMLタグにこの形で指定する。  
```
<button (click)="handleClick">ボタン</button>
```

### モジュール
複数のコンポーネントをまとめるためのもの。  

### Componentにプロパティデータを渡す方法(親コンポーネントから子コンポーネントに値を渡す)
ようはReactのpropsみたいなもの。  
@angular/coreのInputを使って、htmlのプロパティバインディングしたデータを取得する。  
静的なデータ以外にも、もちろんオブジェクト(変数)とかも渡せる。  
```
<app-hoge [fuga]="fuga"></app-hoge>

import { Input } from '@angular/core';

export class Hoge {
	@Input() fuga; // 'fuga'
}
```

### ComponentからEventを受け取る方法(子コンポーネントから親コンポーネントに値を渡す)
https://blog.officekoma.co.jp/2019/05/angulareventemitteroutputemit.html  
@angular/coreのOutputとEventEmitterを使い、メソッドを通じて渡す。  
```
<app-alert (notify)="handleNotify()"></app-alert>

export class AlertComponent implements OnInit {
  @Output() notify = new EventEmitter();
 }
 <button (click)="notify.emit()">Notify</button>
```

### Routingの使い方
ルーティングはRouterModule.forRootの中に定義する。  
app-routing.module.tsでRouterModuleをエクスポートし、アプリ全体で使えるようにする。  
テンプレートファイルにrouter-outletタグを追加することで、ルーティングされたビューを表示する箇所をルーターに教える。  
```
// app.module.ts
@NgModule({
	imports: [
		RouterModule.forRoot([
			{ path: '', component: RootComponent },
			{ path: 'login', component: LoginComponent },
			{ path: 'product/:productId', component: ProductDetail }
		])
	]
})
export class AppModule {}

<a [routerLink]="['/detail']">Login</a>
<a [routerLink]="['/product/1']">Product</a>

// URLによって表示されるコンポーネントが変わる
<router-outlet></router-outlet>
```
#### リダイレクト設定
pathに訪問した際、redirectToにリダイレクトさせる。  
空のpathからリダイレクトさせる場合、pathMatchは'full'にする。  
pathMatchは'prefix'と'full'があり、'prefix'がデフォルト値。  
```
{ path: '', redirectTo: '/dashboard', pathMatch: 'full' }
```


#### URLからパラメータを取得する方法
@angular/routerの ActivatedRoute を使う。  
```
import { ActivatedRoute } from '@angular/router';

constructor(private route: ActivatedRoute) { }

ngOnInit() {
  this.route.paramMap.subscribe(params => {
    console.log(params);
  })
}
```

### AppModule
Angularでは、アプリケーションの部品がどのように合わさるかや、必要なファイルやライブラリを知る必要がある。  
この情報をメタデータという。  
HttpClientModuleやFormsModuleをAppModuleの@NgModuleデコレータに定義しないと使えない。 

### HttpClientを使う
Angularで用意されているhttp通信用のモジュール。  
AppModuleでHttpClientModuleを使うために登録する必要がある。  
```
import { HttpClientModule } from '@angular/common/http';

@NgModule({
	imports: [
		HttpClientModule
	]
})
---
import { HttpClient } from '@angular/common/http';

@Component()
export class HogeWithHttpClient {
	constructor(
		private http: HttpClient
	){}
	
	getJson() {
		this.http.get('/assets/data.json');
	}
}
```

### VScodeでHTMLに警告がでる
テンプレートにHTMLファイルを使っている場合、VSCodeにHTMLHintを入れていると警告がでる。  
.htmlhintrc ファイルでルールを緩和して対策する。  
```
{
  "attr-lowercase": false, // 属性名が小文字か
  "doctype-first": false // DOCTYPE宣言が先頭にあるか
}
```

### Angular CLIを使ったコンポーネントの作成
html,css(scss),specなどを作ってくれる。  
しかもapp.componentにimportや登録まで済ませてくれる。  
```
ng generate component componentName
```

### ビルトインパイプ
補完バインディングの中でパイプ演算子(|)の直後で書式設定ができる。  
文字列、通貨金額、日付、その他の表示データを書式設定するのに向いている。  
また、独自のパイプを作ることもできる。  
```
// 文字列を大文字にするuppercase
{{ 'hogehoge' | uppercase }}
// HOGEHOGE
```

### 双方向データバインディング
[(ngModel)] が双方向データバインディング構文になる。  
```
<input [(ngModel)]="name" />
```

### サービス
親子関係にないコンポーネント同士のデータを共有したりするのに使う。  
サービスのファイルもAngularのコマンドで生成可能。  
```
ng generate service serviceName
```
サービスクラスは@injectable()デコレータで注釈される。  
依存関係注入システムであることを表す。  
@InjectableデコレータのprovidedInでサービスの提供先を指定する。  
CLIで作成した場合、rootがデフォルトになっており、アプリケーション全体で使用することができる。  
providedInで得手のモジュールにのみ適用するようにしておけば、サービスが注入されない場合にツリーシェイキングの対象にできる。  

### その他
**文字列の前に+をつけてるやつ**  
JSの仕様で+を文字列の前におくと数値に暗黙の変換を行う。  