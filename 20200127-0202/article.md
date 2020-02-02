## React
### redux-thunk
ActionCreatorの中で非同期処理を使う。  
単純にActionCreatorの中でアクションを非同期に返しても動かない。  
```
export function asyncIncrement() {
	setTimeout(() => {
		return { type: 'ACTION_TYPE', payload: { hoge: 'hoge' } };
	}, 1000);
}
```
redux-thunkをmiddleWareとして適用する。  
```
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';

const store = createStore(reducer, applyMiddleware(thunk));
```
ActionCreatorで関数を返すようにする。  
actionをそのまま返さず、内部でアクションをdispatchする関数を返すようにする。  
関数がdispatchされてくると、middleWareに使われているredux-thunkが見つけて対応してくれる。  
```
export function asyncIncrement() {
	return dispatch => {
		setTimeout(() => {
			dispatch({ type: 'ACTION_TYPE', payload: { hoge: 'hoge' } });
		}, 1000)
	};
}
```

---

## Next
### getInitialPropsを再実行させたい
リロード以外に再実行させる術はなさそう。  
ベストかわからないが、自分が解決した方法はpropsをstateに持たせて、setStateでクライアント側に更新させる。  
```
const [state, setState] = React.useState<Props>(props);

getInitialProps = {
	return { hoge: 'hoge' }
}
```

---

## その他
- Androidアプリ開発の教科書6~7章
  - https://github.com/y-endo/learn-android-app
- dynamic importの使い方
  - https://github.com/y-endo-sandobox/dynamic-import
- express入門
  - https://github.com/y-endo/learn-express