## Vue/Vuex
### ステートの変更を監視する方法
単純にwatchで監視する。  
```
mounted() {
	this.$store.watch(
		(state, getters) => getters.hoge,
		(newValue, oldValue) => {
			// ~~~ 
		}
	)
}
```

subscribeでミューテーションを購読する。  
全てのミューテーションのあとに実行される。  
```
mounted() {
	this.$store.subscribe((mutation, state) => {
		if (mutation.type === 'HOGE') {
			// ~~~
		}
	})
}
```

subscribeActionでアクションを購読する。  
```
mounted() {
	this.$store.subscribeAction((action, state) => {
		if (action.type === 'HOGE_ACTION') {
			// ~~~
		}
	})
}
```

## AWS
アカウント作成  
https://aws.amazon.com/jp/register-flow/  

代表的なサービス  
### EC2
仮想サーバー  
OSやCore、メモリなどを選ぶタイプなどがある  

### Lambda
JSON形式などのプログラムを登録して、自動的に処理を走らせることができる。  
cronみたいなものかな？  

### S3
ストレージサービス  

### RDS
データベース  

## その他
- Androidアプリ開発の教科書8章
  - オプションメニューとコンテキストメニュー
  - https://github.com/y-endo/learn-android-app