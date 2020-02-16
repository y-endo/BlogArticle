## React
### アニメーション
#### react-transition-group
↓参考記事
https://qiita.com/koedamon/items/2665ea80f19589aa2f7d  

主に { Transition, CSSTransition, TransitionGroup } がある。  
Transitionはインラインスタイルで、コールバックとかもできる。  
CSSTransitionは各フェーズごとにクラスを付与する感じで、Vueのtransitionと同じような感じ。  
TransitionGroupはTransitionまたはCSSTransitionのリストを管理する為のコンポーネント  
コンポーネントのマウント、アンマウント時にenter、exitのスタイルが適用される。  
```
import { Transition, CSSTransition } from 'react-transition-group';

const style = {
  entering: {
    transition: 'opacity 0.5s ease',
    opacity: 1
  },
  entered: {
    transition: '',
    opacity: 1
  },
  exiting: {
    transition: 'opacity 0.5s ease',
    opacity: 0
  },
  exited: {
    transition: '',
    opacity: 0
  }
};
const ExampleTransition = () => {
  const [show, setShow] = useState(true);

  const onClick = () => {
    setShow(!show);
  };

  return (
    <div>
      <Transition in={show} timeout={500}>
        {state => <p style={style[state]}>ExampleTransition</p>}
      </Transition>
      <button onClick={onClick}>On / Off</button>
    </div>
  );
};

const ExampleCSSTransition = () => {
  const [show, setShow] = useState(true);

  const onClick = () => {
    setShow(!show);
  };

  return (
    <div>
      <CSSTransition in={show} classNames={'fade'} timeout={500}>
        <p>ExampleCSSTransition</p>
      </CSSTransition>
      <button onClick={onClick}>On / Off</button>
    </div>
  );
};
```

#### react-spring
↓ 参考記事  
https://qiita.com/uehaj/items/260f188851045cc091ac  
従来のアニメーションライブラリとはちょっと違い、ユニークなアニメーション設定ができる。  
> 従来のアニメーションライブラリだと、アニメーションのタイミングや移動速度などは、継続時間とベジエ曲線(イージング関数)で指定するのが普通でした。これに対してreact-springでは慣性、摩擦力、張力をもった物理的な性質(物性)でタイミングを指定

```
import { useSpring, animated } from 'react-spring';

const Example = () => {
  const [show, setShow] = useState(true);
  const spring = useSpring({
    opacity: show ? 1 : 0
  });

  return (
    <div>
      <animated.div style={spring}>Example</animated.div>
      <button
        onClick={() => {
          setShow(!show);
        }}
      >
        On / Off
      </button>
    </div>
  );
};
```

#### react-transition-groupとstyled-jsxのcssmoduleを組み合わせる方法
```
import transition from "~/styles/components/CSSTransition.scss";
import { CSSTransition } from "react-transition-group";

<CSSTransition
  key={todo.id}
  classNames={{
    enter: transition["fade-enter"],
    enterActive: transition["fade-enter-active"],
    enterDone: transition["fade-enter-done"],
    exit: transition["fade-exit"],
    exitActive: transition["fade-exit-active"],
    exitDone: transition["fade-exit-done"]
  }}
  timeout={500}
>
  <ToDoListItem todo={todo} />
</CSSTransition>;
```

## その他
- Androidアプリ開発の教科書9章
  - フラグメントの使い方
  - https://github.com/y-endo/learn-android-app