## NEXT.js
### タイトルの変更
https://stackoverflow.com/questions/52170634/how-to-set-documents-title-per-page  

### styled-jsxでsassを使う  
styled-jsx-plugin-sass  
https://github.com/giuseppeg/styled-jsx-plugin-sass  

babel.config.jsの追記例  
https://github.com/zeit/styled-jsx/issues/569  

### getInitialPropsでuseRouterを使いたい。
普通に実装するとエラーがでる。  
https://stackoverflow.com/questions/58407074/userouter-not-working-inside-getinitialprops  
getInitialPropsはサーバー側で動いているから当然。consoleもターミナル側に表示される。

### dynamic routingとLink組み合わせ
```
/task/[id].tsx

// これでも動作するけどハードリフレッシュになる
<Link href="/task/1">
<a>~~~</a>
</Link>

// こうする
<Link href="/task/[id]" as="/task/1">
<a>~~~</a>
</Link>
```

### APIルートにGraphQL
https://qiita.com/NanimonoDaemon/items/a0ed3d3b8a93b306c88c

---

## TypeScript
### React.js
React + TypeScript入門記事  
https://www.webopixel.net/javascript/1588.html  

React Event + TypeScript  
https://qiita.com/hikonaz/items/5d2a526a217e05162a0a  

Hooks + TypeScript  
https://fettblog.eu/typescript-react/hooks/  

### Next.js
環境構築  
https://qiita.com/matkatsu8/items/f0a592f713e68a8d95b7  

---

## Kotlin
### Kotlin入門
Udemyの入門講座を購入したのでやっていく。  
https://www.udemy.com/course/kotlin-beginner/  

### 基本構文
#### 変数
TypeScriptのlet/constがvarになった印象。  
型推論があるから、初期値があるなら型の省略できる。  
```
var 変数名: 変数の型 = 初期値
```

型サフィックスについて  
文字や値にサフィックスを付けることで、型を定義できる。  
```
var a = 10L // Long型になる
var b = 10F // Float型になる
```

#### リテラル
trimMarginを使うと、|が行頭になる。  
```
println("改行する\n改行した")
println("""改行する
					|改行した""").trimMargin()
```

#### 配列とコレクション
配列と代表的なコレクション（List・Set・Map）  
配列は宣言の段階でサイズを決める必要があるので、柔軟性に欠ける。  
```
var array = arrayOf(1, 2, 3) // [1, 2, 3]
var intArray = intArrayOf(1, 2, 3) // [1, 2, 3]
var strArray: Array<String?> = arrayOfNulls(3) // [null, null, null]

var list = listOf("a", "b", "c")
var set = setOf("A", "B", "A", "C")
var map = mapOf("First" to 1)
```

### 定数
定数は val で宣言する。  
```
val a = 10
```

---

## GraphQL
### GraphQLの入門記事
https://tatsuyashi.hatenablog.com/entry/2019/01/15/005225  
https://qiita.com/zonomaa/items/5de4b14dcd839db5f148  
https://cloudnweb.dev/2019/06/graphql-with-apollo-server-and-express-graphql-series-part-1/  
https://employment.en-japan.com/engineerhub/entry/2018/12/26/103000  

### APIの種類

- Query（データ取得）
- Mutation（データ更新）
- Subscription（変更検知）

これらはGraphQLスキーマの予約後になっている。  
```
type Query{}
type Mutaion{}
type Subscription{}
```

### ざっくりとした自分の中での理解
apollo-server-expressを使ってる。  

- スキーマの定義
	- typeDefsで型を定義する。定義した型は上記のQueryとかで使う
- リゾルバを定義
	- スキーマで定義したQueryとかに対応するリゾルバをresolversに定義する。

```
const users = [
	{ id: 1, name: 'Hoge' },
	{ id: 2, name: 'fuga' }
];

const typeDefs = gql`
	type User {
		id: Int!
		name: String!
	}
	
	type Query {
		users: [User]
	}
`;

const resolvers = {
	Query: {
		users: () => users
	}
};
```

### query
データを取得するqueryの発行  
取得するデータのフィールドを空にはできない。  
```
// bad
query {
	data
}

// good
query {
	data {
		hoge
		fuga
	}
}
```

### Apollo Client + React 入門
https://www.apollographql.com/docs/react/  
https://qiita.com/seya/items/e1d8e77352239c4c4897  

- apollo-boost
	- その名の通り「とりあえずApolloで動かしたいんや！！」というあなたをブーストしてくれるパッケージです。
	- apollo-cache-inmemory や apollo-link-http などApollo Clientを使う際に基本使うことになるであろうパッケージたちが一緒にインストールされます。詳しい中身を知りたい方はREADMEをどうぞ。
- react-apollo
	- ReactとGraphQLクエリの繋ぎこみをサポートしてくれるライブラリです。
- graphql-tag
	- GraphQLクエリをテンプレートリテラルで書いたものをパースしてくれる便利なライブラリです。
- graphql
	- graphql-tagのpeerDependencyとして必要です。他にも色々機能あるパッケージなんですが、今回入れる目的はそれ。

@apollo/clientだけで充分じゃない？  
公式チュートリアルをやった感じだとreact-apolloとかgraphql-tagは必要なくて、@apollo/clientに全て含まれてそう。

### apollo-boost はサンプルコード専用と思ったほうがよい
https://gfx.hatenablog.com/entry/2018/10/30/112054  
apollo-boostはサンプルとかで使って、プロダクションではapollo-clientおよび関連モジュールを直接使ったほうがよい。  

---

## MongoDB
### MongoDBをbrewでMacにインストール
https://reffect.co.jp/windows/mac-mongodb-install  
設定ファイル  
/usr/local/etc/mongod.conf  
MongoDBのデフォルトのポートは、27017に設定されています。  

### NodeとMongoDBをつなげるやつ
https://www.npmjs.com/package/mongodb  
```
const mongodb = require('mongodb');
const MongoClient = mongodb.MongoClient;

MongoClient.connect('mongodb://localhost:27017', (error, client) => {
  if (error) throw error;
  console.log('Connected successfully to server');

  const database = client.db('databaseName');
  database
    .collection('collectionName')
    .find(filter)
    .toArray((error, response) => {
      if (error) throw error;
      
      console.log(response);

      client.close();
    });
});
```

### MongoDBでasync/await
https://qiita.com/junjis0203/items/3fd47eee226acc193a1c  
https://qiita.com/sl2/items/681d40e4ba018ea8b808  

### MongoDBのGUI（管理ツール）
Compass（公式）  
https://www.mongodb.com/download-center/compass  

### GraphQLとMongoDBをつなげる
mongoose  
https://mongoosejs.com/

mongooseのSchemaにコレクションを指定する。  
https://stackoverflow.com/questions/7486528/mongoose-force-collection-name  
```
new mongoose.Schema({}, { collection: 'collectionName' });
```