# useRef

결론부터 말하자면 useRef는 변화는 감지해야 하지만, 그 변화가 렌더링을 발생시키면 안되는 값을 다룰때 정말 편리하다.

Ref는 컴포넌트 내에서 일반적인 변수와 다르게 컴포넌트 생명주기와 같이 작동한다.

즉, 렌더링 되어도 Ref값은 컴포넌트가 사라지기 전까지 그 값이 유지된다.

```javascript
import React, { useState, useRef, useEffect } from "react";

const App = () => {
  const [count, setCount] = useState(1);
  const renderCount = useRef(1);

  useEffect(() => {
    renderCount.current = renderCount.current + 1;
    console.log("렌더링 수: ", renderCount.current);
  });

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>올려</button>
    </div>
  );
};

export default App;
```

다시 한번 기억하자 Ref는 값이 변화해도 렌더링을 발생시키지 않는다.

위 점을 유의하면서 useEffect 부분을 다시 살펴보자.

1. useEffect에 의해 Ref값인 renderCount의 값이 1증가한다.
2. Ref의 값 변화가 더 이상 렌더링을 일으키지 않는다.

```javascript
import React, { useState, useRef, useEffect } from "react";

const App = () => {
  const [count, setCount] = useState(1);
  const [renderCount, setRenderCount] = useState(1);

  useEffect(() => {
    setRenderCount(renderCount + 1);
  });

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>올려</button>
    </div>
  );
};
```

위의 함수에서는 useEffect로 렌더링 될때 setRenderCount가 실행된다.

따라서 useState에 대한 renderCount의 값이 변화함으로 다시 렌더링 된다.

다시 렌더링 됨에 따라 또다시 useEffect에 의해 무한루프에 빠지게 된다.

> 대표적인 useCase

- TextBox에 Focus를 줄 때 사용한다. Ref를 사용해 DOM 요소에 접근이 가능하다.

```javascript
import React, { useEffect, useRef } from "react";

const App = () => {
  const inputRef = useRef();

  useEffect(() => {
    // console.log(inputRef);
    inputRef.current.focus();
  }, []);

  const login = () => {
    alert(`환영합니다. ${inputRef.current.value}!`);
    inputRef.current.focus();
  };
  return (
    <div>
      <input ref={inputRef} type="text" placeholder="username" />
      <button onClick={login}>로그인</button>
    </div>
  );
};

export default App;
```

useEffect(두번째 인자 빈 배열)에 의해 처음 rendering시에만 inputRef.current값에 focus가 된다.

```javascript
<input ref={inputRef} type="text" placeholder="username" />
```

이렇게 코드를 작성하면 inputRef의 current값은 HTML input태그를 가르키게 된다.
따라서 위에서 inputRef.current를 변수처럼 사용할 수 있다.
