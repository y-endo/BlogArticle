## React
#### react-spring
**単一コンポーネントにuseTransitionを使う マウント/アンマウントでアニメーション**  
https://alligator.io/react/advanced-react-spring/  
```
import { useTransition, animated } from 'react-spring';

const Index: React.FC = () => {
  const [isModalOpen, setIsModalOpen] = React.useState(false);
  const transition = useTransition(isModalOpen, null, {
    from: { opacity: 0 },
    enter: { opacity: 1 },
    leave: { opacity: 0 }
  });
  
  return <>
          {transition.map(
            ({ item, key, props }) =>
              item && (
                <animated.div style={props} key={key}>
                  <Modal />
                </animated.div>
              )
          )}
        </>;
};
```
### コード分割
https://ja.reactjs.org/docs/code-splitting.html  
dynamic importに対応しているし、React.lazyを使えばもっと簡単にコンポーネントの遅延読み込みができる。  
ただし、React.lazyはSSRには使用できない。  

#### React.lazy
```
import OtherComponent from './OtherComponent';
↓
const OtherComponent = React.lazy(() => import('./OtherComponent'));
```

#### Suspense
遅延コンポーネントは、Suspenseコンポーネント内でレンダーされる必要がある。  
これによって、遅延コンポーネントのローディング待機中にフォールバック用のコンテンツを表示できる。  
```
<div>
	<Suspense fallback={<div>Loading...</div>}>
		<OtherComponent />
	</Suspense>
</div>
```