## React

### Functinal Componentから呼ぶ関数はどこで定義するべきか
https://stackoverflow.com/questions/46138145/functions-in-stateless-components  
コンポーネントの中に入れてしまうと、レンダリングする度に関数が再定義されるので、そとで定義するべき。  

### typescript-fsa
fsaは「Flux Standard Action」の略で、Fluxにおけるアクションの形式を規定するものです。このfsaをTypescriptでシンプルに記述ができるライブラリが、typescript-fsa typescript-fsa-reducersです。typescript-fsaはAction側、typescript-fsa-reducersがReducer側で利用するライブラリです。  

### React.FCで子コンポーネントの関数を呼びたいとき。
https://ja.reactjs.org/docs/hooks-reference.html#useimperativehandle 

### Functinal Componentのインスタンス変数
再レンダリングで保持したい変数（stateじゃない）はuseRefを使う。  
https://ja.reactjs.org/docs/hooks-faq.html#is-there-something-like-instance-variables  
> はい！ useRef() フックは DOM への参照を保持するためだけにあるのではありません。“ref” オブジェクトは汎用のコンテナであり、その current プロパティの値は変更可能かつどのような値でも保持することができますので、クラスのインスタンス変数と同様に利用できます。

ただし、TypeScriptで書く場合currentがreadOnlyになる場合がある。  
https://stackoverflow.com/questions/58017215/what-typescript-type-do-i-use-with-useref-hook-when-setting-current-manually  
```
const hoge = useRef<HTMLElement>(null); // ← hoge.current = read-only
const hoge = useRef<HTMLElement | null>(null); // OK
```

---

## Next

### mongoose（MongoDB）がhot reloadでバグるときの対策 
https://www.hoangvvo.com/blog/migrate-from-express-js-to-next-js-api-routes/  
既にコンパイルされたmongoose.modelを上書きできない。  
```
const Usert = mongoose.models.User || mongoose.model('User', UserSchema);
```

### ディレクトリ構成 参考
https://sergiodxa.com/articles/next-file-structure/  
Next.js開発者のZeitがこうしてる？  

### APIルートでSubscription
無理っぽい。  
別途サーバーを用意しないとできない？多分。  
queryのキャッシュを無効にして、更新する度にquery発行で凌ぐ

### APIルートでfsをつかう
__dirnameが正常に動かないので、fs.readFileがうまくいかない。  
https://github.com/zeit/next.js/issues/8251  
next.config.jsにプロジェクトのルートパスをもたせる。

---

## GraphQL

### fragment
冗長な繰り返しを避けるために、プロパティをまとめる  
```
fragment allProps on prop {
	id
	name
	age
}

query {
	users {
		allProps
	}
}
```
Apollo Clientのfragmentの使い方  
https://www.apollographql.com/docs/react/data/fragments/  

### 入力値バリデーション
更新する際に、バリデーションを事前に定義することが可能です。$文字列の後に続くものが変数となり、:の後に型と必須チェックを記載しています。  
例えば、文字列型で必須にしたい場合は$str:String!、数値型で必須ではない場合は$num:Intとなります。  
```
mutation setStatus($id:ID! $status:LiftStatus!) {
  setLiftStatus(id:$id status: $status) {
    id
    name
    status
  }
}
```

Apollo Subscriptionの参考記事  
https://lambda4.fun/graphql/apollo-express/subscription/  
```
const express = require('express');
const { ApolloServer, PubSub } = require('apollo-server-express');
const http = require('http');
const typeDefs = require('./typedefs');
const resolvers = require('./resolvers');

const app = express();
const port = 3030;

const server = new ApolloServer({
  typeDefs,
  resolvers,
  context: { pubsub: new PubSub() }
});
server.applyMiddleware({ app, path: '/graphql' });

const httpServer = http.createServer(app);
server.installSubscriptionHandlers(httpServer);

httpServer.listen({ port }, () => {
  console.log(`Server ready at http://localhost:${port}${server.graphqlPath}`);
});

---

Mutation: {
  addUser: async (_, args, context) => {
    const user = new UserModel({ ...args });
    await user.save();
    context.pubsub.publish('userAdded', {
      userAdded: {
        id: user.id,
        name: user.name,
        age: user.age
      }
    });
    return user;
  }
},
Subscription: {
  userAdded: {
    subscribe: (_, __, context) => context.pubsub.asyncIterator('userAdded')
  }
}
```

### Typeのextend
https://www.apollographql.com/docs/graphql-tools/generate-schema/#extending-types  
```
type Hoge {
	hoge: String
}
extend type Hoge {
	fuga: String
}
```

### 引数の型をObject（定義した型）にしたい
typeで宣言したものを、引数の型に指定するとエラーになる。  
inputで宣言する。  
```
type Hoge {}
input HogeInput {}

Mutation {
	hoge(hoge: Hoge) {} # エラー
	hoge(hoge: HogeInput) {} # OK
}
```

### graphql-codegen
TypeScriptの型定義ファイルを、スキーマから自動生成する。  
https://graphql-code-generator.com/  
https://qiita.com/mizchi/items/fb9f598cea94d2c8072d

### webpackでgraphqlファイルを読み込む
https://dev.to/open-graphql/how-to-resolve-import-for-the-graphql-file-with-typescript-and-webpack-35lf

---

## MongoDB
### コマンド
```
mongo
show dbs
db
db.createCollection('collectionName')

db.colleciton.insert()

db.collection.find()
db.collection.find(query)

第2引数で取得結果を絞れる、0 or 1 で取得するフィールドを制御
db.collection.find({}, {_id: 0, name: 1})

db.collection.update({検索条件}, {更新内容})
e.g. db.collection.update({ name: 'AAA' }, { age: 30 })
注意）updateはごそっと上書きなので、上記の例だと { age: 30 } だけになる。
特定のフィールドだけ更新したい場合は、$set修飾子を使う
db.collection.update({ name: 'AAA' }, { $set: { age: 30 } })

db.collection.remove()
db.collection.remove(query)
```

### mongooseでの保存操作
```
const hoge = new HogeModel({ name: 'fuga' });
hoge.save()

// 大量のドキュメントを挿入する場合
HogeModel.insertMany([{ name: 'fuga' }, { name: 'hoge' }]);
```

mongoose経由でデータを追加すると、versionKeyが追加される。  
フィールド名を変えることができて、デフォルトは__v  
https://intellipaat.com/community/27868/what-is-the-v-field-in-mongoose

---

## Docker
読んだ。  
https://knowledge.sakura.ad.jp/13265/  

### イメージ
コンテナを作る元、自分で作るかDockerHubからダウンロードする  

### コンテナ
イメージから作成するコンテナ、サーバ的なもん？  

### Dockerfile
イメージの設計書てきなもの？  
このファイルの設定を元にイメージを作成できる  

### コマンド
```
#イメージ一覧
docker images

#コンテナ一覧
docker ps
#コンテナ一覧（停止中も含める）
docker ps -a

#コンテナを作成
docker create イメージ名
#コンテナ名を指定して作成
docker create --name コンテナ名 イメージ名

#コンテナ起動
docker start ID or コンテナ名

#コンテナにログイン
docker exec -it コンテナ名 bash

#コンテナ終了
docker stop ID or コンテナ名
docker kill ID or コンテナ名
killは即時終了、多分

#コンテナ削除
docker rm ID or コンテナ名

#イメージからコンテナを起動
docker run hello-world

#イメージのダウンロードだけ
docker pull?hello-world

#イメージの削除
docker rmi ID or イメージ名

#Dockerfileからイメージを作成
docker build Dockerfileのパス

#コンテナからホストへのファイルコピー
docker cp <コンテナID>:/etc/my.cnf my.cnf
```

#### docker run オプション
- -d: バックグラウンドで行う。
- -p: ポートの指定。ローカルポート:コンテナポート
- -v: ローカルマシンのディレクトリマウント
	- docker run -v "マウントするディレクトリ:マウント先のパス"

### Docker Compose
複数のコンテナをまとめる。  

```
#docker-compose.ymlの設定に従ってコンテナを作成・起動する
docker-compose up -d

#コンテナ削除
docker-compose down

#Dockerfileから作成する場合
docker-compose up -d --build

#開始
docker-compose start

#終了
docker-compose stop
```

---

## TypeScript

### 型ガードのスコープ問題
入れ子になったり、スコープが外れると型ガードの定義がはずれる。  
その対策。  
https://www.nechai.net/2016/10/05/overcoming-typescripts-type-guards-limitations-in-the-nested-scope/  

### 型についてめっちゃ詳しくまとめてる記事
https://nju33.com/note?note=typescript 