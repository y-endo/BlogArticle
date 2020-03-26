## MongoDB / Mongoose
### ドキュメント内の配列に要素を追加する
http://sekitaka-1214.hatenablog.com/entry/2014/05/04/102225  
https://docs.mongodb.com/manual/reference/operator/update/push/  
```
// $push を使う
db.updateOne({ id: 'hoge' }, { $push: { hoge: 'fuga' } })
```

## React
### @apollo/react-hooks
#### useQueryのrefetch
useQueryを再レンダリングとかではなく、もう一度データを取得させたい場合はrefetchを使う。  
```
import { useQuery } from '@apollo/react-hooks';

const { refetch } = useQuery();

refetch();
```

なぜかは分かっていないが、useQueryを使ったページから別のページに遷移して、refetchを行うと、strictmodeの場合警告が出る。（アンマウントしたコンポーネントに対するステート変更）  
useLazyQueryに置き換えると治ったので、一旦置き換えで対処する。