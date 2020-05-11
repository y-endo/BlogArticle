## Angular
### Form
#### リアクティブフォームバリデーション
リアクティブフォームのバリデーションはテンプレートの属性を使わずに、バリデータ関数をコンポーネントクラスのフォームコントロールモデルに直接追加する。  
**ブラウザのバリデーションを有効にするには**  
formタグに「ngNativeValidate」属性を追加する。これがないとrequired属性を設定していてもブラウザのアラートが表示されない。  
```
<form ngNativeValidate>
<input required>
</form>
```

##### バリデータ関数
バリデータ関数には同期バリデータと非同期バリデータの2種類がある。  
同期バリデータはFormControlインスタンス時の第2引数、非同期バリデータは第3引数に渡す。  

##### 組み込みバリデータ
requiredやminlengthなど、テンプレート駆動形フォームの属性として使用できる組み込みバリデータは、@angular/formsのValidatorsクラスの関数として使用できる。  
https://angular.jp/api/forms/Validators  
また独自のバリデータ関数を記述することも可能。  
https://angular.jp/guide/form-validation#custom-validators  
複数のバリデータを設定したい場合は、配列で指定する。  
```
'title': new FormControl('title', Validators.required)
'title': new FormControl('title', [Validators.required, Validators.minlength(5)])
```
formGroup.get('プロパティ名').コントロールステータス で状態を知ることができる。  
```
this.fb.get('name').valid
this.fb.get('name').dirty
...etc
```
バリデーションにエラーがある場合はerrorsでアクセスできる。  
```
this.fb.get('name').errors
```

##### コントロールステータスCSSクラス
フォームの状態に応じてクラスが付与される。  
- ng-valid: 値が妥当な場合
- ng-invalid: 値が不正な場合
- ng-pristine: 値が変更されていない場合
- ng-dirty: 値が変更された場合
- ng-pending: バリデータの検証作業待ち
- ng-untouched: 要素にフォーカスが当たったことがある場合
- ng-touched: 要素にフォーカスが当たったことがある場合