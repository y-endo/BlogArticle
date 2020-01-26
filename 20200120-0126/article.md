## Kotlin
### 演算子
#### 範囲演算子
範囲を表現できる。  
```
// m..n
val i = 10
println(i in 1..20) // true
```

#### ifは結果を変数に格納できる
```
var num = 10;
var msg = if (num >= 5) {
	"numは5以上"
} else {
	"numは5未満"
}
println(msg) // numは5以上
```

#### when式
switchみたいなもん。  
whenもifと同様に結果を返す。  
```
when(チェックする変数・式) {
	条件1 -> 条件1を満たしたときの処理
	条件2 -> 条件2を満たしたときの処理
	else -> {
		条件を満たさなかったときの処理
	}
}
```
引数がなければ、ifの代替として使える。  
```
val a = 10
when {
	a <= 5 -> println("aは5以下")
	a <= 10 -> println("aは10以下")
	else -> println("aは10よりも大きい")
}
```

#### forループ
ラベル構文+breakで外側のループも終了できる。  
outer@のループをbreakするには、break@outer  
```
outer@ for (i in 1..3) {
	for (j in 1..3) {
		if (i * j > 5) break@outer
		print("${i * j}")
	}
	println()
}

結果
1 2 3
2 4
```

### 関数
#### 構文  
```
fun 関数名 (引数: 引数の型, 引数2: 引数の型 = デフォルト値, ...): 戻り値の型 {
	// 処理
}

関数名(引数)
```
引数と戻り値の型宣言は必須。  
戻り値がない場合は、Unitを指定する。  

戻り値が単一の場合は構文を省略した書き方がができる。  
```
fun sayHello(): Unit = println("Hello")
sayHello()
```

#### 名前付き引数
関数を呼び出すときに、名前付き引数を使えばどの引数にどの値を渡すか指定できる  
名前付き引数と通常の引数を混在させる場合は、名前付き引数を後方に記述する必要がある。  
```
fun sayHello(firstName: String = "Hoge", lastName: String = "Fuga"): Unit {
	println("Hello, ${firstName} ${lastName}")
}

sayHello(lastName = "Endo") // Hello Fuga Endo
sayHello("Yuki", lastName = "Endo") // Hello Yuki Endo
sayHello(firstName = "Yuki", "Endo") // エラー
```

#### 可変長引数
引数にvarargを使うと可変長引数になる。  
複数の値を渡せる。  
内部的には配列と同じ扱いになる。  
```
fun allSum(vararg values: Int): Int {
	var result = 0
	for (value in values) {
		result += value
	}
	return result
}
allSum(1, 2, 3, 4, 5) // 15
```

#### 高階関数
引数に関数を受け取ったり、戻り値で関数を返す関数。  
forEachなど。  
引数に関数を渡すには「::関数名」という形で渡す。  
```
fun prnt(n: Int) {
	print(n)
}

arrayOf(1, 2, 3, 4).forEach(::prnt)
```

#### 匿名関数とラムダ式
匿名関数（無名関数）の書き方は、ラムダ式が主流  
```
// ラムダ式の書き方
{ 引数 -> 処理 }

// 省略形がたくさんある
最後の引数がラムダ式の場合は()の外に出せる
fun hoge(a: Int, b: () -> Unit){}
hoge(1) {
	// ラムダ式
}
```

ラムダ式でreturnすると直上の関数を抜けてしまう。  
```
fun hoge() {
	val arr = arrayOf(1,2,3)
	arr.foEach {
		return // このreturnはforEachではなく、hogeを抜ける
	}
}
```
それを防ぐためにラベルを使う。  
```
fun hoge() {
	val arr = arrayOf(1,2,3)
	arr.foEach loop@ {
		return@loop
	}
}
```
ラムダ式を引数にとる高階関数の名前をラベルに指定してもよい  
```
fun hoge() {
	val arr = arrayOf(1,2,3)
	arr.foEach {
		return@forEach
	}
}
```

### クラス
#### プライマリコンストラクタ
Kotlinのコンストラクタ構文  
```
class クラス名 constructor(引数: 型) {}
```
コンストラクタで受け取った引数はクラス内のinitで扱われる。  
```
class Human constructor(name: String, age: Int) {
	var name: String
	var age: Int
	
	init {
		this.name = name
		this.age = age
	}
}
```
プライマリコンストラクタがアノテーションやアクセス修飾子を持たない場合はconstructorを省略可能。  
```
class Human(name: String, age: Int) {}
```

プライマリコンストラクタの引数にvar/valをつけることでプロパティの宣言と初期化を同時に行える。  
```
class Human(var name: String, var age: Int) {
	fun intro() {
		println("${this.name}、${this.age}")
	}
}
```

#### クラスの継承
Kotlinでクラスの継承を行う場合、元となるクラスはopenと宣言する必要がある。  
継承するときは クラス名の右に :元のクラス名 と書く。  
この時に引数を渡すとsuper()的な感じで、元のクラスのコンストラクタを通すことができる。  
```
open class Human(var name: String) {
	open fun intro() {
		println("")
	}
}

class PerfectHuman(name: String, var place: String): Human(name) {
	override fun intro() {
		println("")
		super.intro()
	}
}
```

#### データクラス
データを保持するだけの特殊なクラス。  
データクラスを定義する際のルール。  
- プライマリコンストラクタが1つ以上の引数を持つ
- コンストラクタの引数はすべてvar/valを付与してプロパティの宣言とする
- abstract / open / saled / innerにすることはできない

よく使うメソッドが予め用意されている。
- equals
- toString
- componentN
- copy
など

```
data class Member(val name: String, val age: Int)
```

#### オブジェクト式
再利用を目的としないクラス。  
```
objet {クラス本体}
```

#### オブジェクト宣言
ひとつのインスタンスしか持たないようなクラス（シングルトン）
```
object オブジェクト名 {オブジェクト本体}
object TanakaTaro {
	val name = "田中太郎"
	
	fun intro() {
		println("${name}です。")
	}
}
```

#### コンパニオンオブジェクト
staticなクラス変数・メソッドの事
```
class MyClass {
	companion object {
		fun create(): MyClass = MyClass()
	}
}

val myClass = MyClass.create()
println(my::class) // class MyClass
```

#### コンパイル時定数
コンパイル時に初期化される定数。  
const修飾子を付与する。  
const修飾子を利用するには以下の条件を満たす必要がある。  
- トップレベルかオブジェクト式、コンパニオンオブジェクトのメンバである
- 整数、浮動小数点数、真偽値、文字型、文字列型のいずれかで初期化されている
- カスタムのgetterを持たない
```
const val VERSION = 1
```

#### ネストクラス
classの中に定義するclass  
親に強く依存していて、親からしか呼ばれないものはネストクラスを利用する。  
```
class Outer {
	private class Nested {
		fun hoge()
	}
}
```
ネストされたクラスから親クラスを参照したい場合は、inner修飾子を使う。  
```
class Outer(val name: String = "Outer") {
	inner class Nested {
		fun hoge() {
			println("${this@Outer.name}")
		}
	}
}
```

### 拡張関数
継承を用いずに既存のクラスにメソッドを追加できる。  
拡張関数を使えば、openではないクラスにたいしてもメソッドを追加できるようになる。  
```
fun 拡張するクラス名.追加メソッド名(引数: 型, ...): 型 {処理}
```

### オーバーロード
引数の方や数が異なっていれば同じ名前のメソッドを定義することができる。  
これをメソッドのオーバーロードという。  
基本的に演算子以外では使わない。

## Vue
### Vuexの基本
Storeにstate,getters,mutations,actionsを定義する。  
Storeを注入したコンポーネント及び、子コンポーネントがStoreにアクセスできる。  

- state
	- Storeで管理するデータ
- getters
	- state内のデータの状態から算出される値
- mutations
	- stateのデータを変更する関数（非同期処理は不可）
- actions
	- コンポーネントから値を受け取って、mutationsにわたす（非同期が可能）。Promiseが返ってくる

### ステート
コンポーネントのcomputedから取得する。  
```
computed: {
	count() {
		return this.$store.state.count
	}
}
```
mapStateヘルパー  
```
import { mapState } from 'vuex';

export default {
	// mapStateの中では第一引数にstateが入ってくる
	computed: mapState({
		count: state => state.count,
		countAlias: 'count' // (state.count)
	})
}
```
算出プロパティの名前がステートサブツリーの名前と同じ場合は、文字列配列をmapStateに渡せる。  
```
computed: mapState([
	'count',
	'member'
])
```
他のローカル算出プロパティと組み合わせる場合  
```
computed: {
	localComputed () {},
	...mapState([])
}
```

### ゲッター
Store側に持たせるcomputedで、各コンポーネントから呼び出せる。  
ゲッターは第1引数にstateを受け取る。  
```
new Vuex.Store({
	state: {
		count: 1
	},
	getters: {
		countPlusOne: state => {
			return state.count + 1;
		}
	}
})
```
コンポーネントのcomputedから取得する。  
```
computed: {
	getCount() {
		return this.$store.getters.countPlusOne
	}
}
```
mapGettersヘルパー  
```
import { mapGetters } from 'vuex';

computed: {
	...mapGetters([
		'countPlusOne'
	])
}
```

### ミューテーション
```
new Vuex.Store({
	state: {
		count: 1
	},
	mutations: {
		increment (state) {
			state.count++;
		},
		hoge: (state, payload) {
			state.count += payload.amount;
		}
	}
});

store.commit('increment');
store.commit('hoge', {
	amount: 10
})
```

### アクション
ミューテーションに似ている。  
- アクションは状態を変更するのではなく、ミューテーションをコミットする。
- アクションは任意の非同期処理を含むことができる。

```
new Vuex.Store({
	state: {
		count: 1
	},
	mutations: {
		increment (state, payload) {
			state.count += payload.amount;
		}
	},
	actions: {
		incrementAsync ({ commit }) {
			setTimeout(() => {
				commit('increment', 10);
			}, 1000);
		}
	}
});

export default {
	methods: {
		...mapActions([
			'increment'
		])
	}
}
```
mapActionsヘルパー  
```
methods: {
	...mapActions(['increment'])
}

<button @click=increment({ hoge: 'fuga' })>
```

## Nuxt

### SSRでデータ通信
コンポーネントのasyncDataを使う。  
https://ja.nuxtjs.org/guide/async-data  
クライアント側、サーバ側どちらで動いているか判定するには  
```
asyncData(context) {
	if (process.server) // sever side
	if (process.client) // client side 
}
```

## Android
### xmlns:androidとは
LinearLayoutとかに設定している以下の属性はなにか。  
xmlns:android="http://schemas.android.com/apk/res/android"  
xmlns とは、名前空間（NameSpace)の宣言のことです。このXML文書で使う用語はAndroidの定義に従います、という意味です。
