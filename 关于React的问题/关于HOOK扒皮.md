
## HOOK

 Hook 是一些可以让你在函数组件里“钩入” React state 及生命周期等特性的函数。
 一句话就是，它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性
 
  * 只能在函数最外层调用 Hook。不要在循环、条件判断或者子函数中调用，只在React函数中调用
  * 用只能在 React 的【函数组件】和【自定义Hook】中调用 Hook。
  * Hook 的每次调用都有一个完全独立的 state 
  
  
 ## State hook
 在我们之前的React中只有class组件可以拥有内部state，现在一个函数组件中，通过useState这个State hook，来维护一个内部state
 
 * useState()调用的时候定义了一个state。
 * useState()的唯一参数就是初始state值。
 * useState()当前 state 以及更新 state 的函数。
 
 * 它类似 class 组件的 this.setState，但是它不会把新的 state 和旧的 state 进行合并而是进行替换。
 
 `
   // 声明一个叫 "count" 的 state 变量，React 会在重复渲染时记住它当前的值，并且提供最新的值给我们的函数
  const [count, setCount] = useState(0);
 `
 
 `
import React, { useState } from 'react';
function Example() {
  // 声明一个叫 “count” 的 state 变量。
  const [count, setCount] = useState(0);
  // 声明多个 state 变量！
  const [age, setAge] = useState(42);
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);
  // ...
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        点我加count
      </button>
      <button onClick={() => setAge(age + 1)}>
        age
      </button>
      // ...
    </div>
  );
}
 `
 
 ### 当使用多个useState()时，react怎么知道哪个state 对应哪个 useState？
 
 - React 靠的是 Hook 调用的顺序
 `
     function Form() {
      // 1. Use the name state variable
      const [name, setName] = useState('Mary');
      // 2. Use an effect for persisting the form
      useEffect(function persistForm() {
        localStorage.setItem('formData', name);
      });
      // 3. Use the surname state variable
      const [surname, setSurname] = useState('Poppins');
      // 4. Use an effect for updating the title
      useEffect(function updateTitle() {
        document.title = name + ' ' + surname;
      });
      // ...
    }
    // ------------
    // 首次渲染
    // ------------
    useState('Mary')           // 1. 使用 'Mary' 初始化变量名为 name 的 state
    useEffect(persistForm)     // 2. 添加 effect 以保存 form 操作
    useState('Poppins')        // 3. 使用 'Poppins' 初始化变量名为 surname 的 state
    useEffect(updateTitle)     // 4. 添加 effect 以更新标题
    // -------------
    // 二次渲染
    // -------------
    useState('Mary')           // 1. 读取变量名为 name 的 state（参数被忽略）
    useEffect(persistForm)     // 2. 替换保存 form 的 effect
    useState('Poppins')        // 3. 读取变量名为 surname 的 state（参数被忽略）
    useEffect(updateTitle)     // 4. 替换更新标题的 effect
    // ...
 `
 
 
 ## Effect hook
 
  React 组件中执行过数据获取、订阅或者手动修改过 DOM。我们统一把这些操作称为“副作用”，或者简称为“作用”。
  在React class 中，我们把副作用操作放到 componentDidMount 和 componentDidUpdate 函数中，
  useEffect， 就是一个Effect hook，它跟 class 组件中的 componentDidMount、componentDidUpdate 和 componentWillUnmount 具有相同的用途，只不过被合并成了一个 API。
  effect 的清除机制可以避免 componentDidUpdate 和 componentWillUnmount 中的重复，更可以按照功能去分隔他们

  * 默认情况下，React 会在每次渲染后调用副作用函数 —— 包括第一次渲染的时候，通过使用这个 Hook，你可以告诉 React 组件需要在渲染后执行某些操作
  * 副作用函数是在组件内声明的，所以它们可以访问到组件的 props 和 state（闭包大法好）。
  * Effect通过每次更新执行，可以通过返回一个函数来指定如何“清除”副作用（class中使用componentWillUnmount中去取消这个事件）
  * React 会等待浏览器完成画面渲染之后才会延迟调用 useEffect，异步的，不阻塞浏览器更新屏幕
  * 如果想执行只运行一次的 effect（仅在组件挂载和卸载时执行），可以传递一个空数组（[]）
  
  `
  import React, { useState, useEffect } from 'react';
  function Example() {
      const [count, setConnt] = useState(0)
      // 相当于 componentDidMount 和 componentDidUpdate:
      useEffect(() => {
        // 使用浏览器的 API 更新页面标题
        document.title = `You clicked ${count} times`;
      });
      useEffect(() => {
         //  比如加了一个订阅
         return (() => {
           // 取消这个订阅了
         })
      })
      return (
        <div>
          <p>You clicked {count} times</p>
          <button onClick={() => setCount(count + 1)}>
            Click me
          </button>
        </div>
      );
   }
  `
  
  与 componentDidMount 或 componentDidUpdate 不同，使用 useEffect 调度的 effect 不会阻塞浏览器更新屏幕，这让你的应用看起来响应更快。
  大多数情况下，effect 不需要同步地执行
  
  ### 关注点分离：
  在class中我们需要在不同的生命周期去处理同一件事情，比如设置 document.title 的逻辑是如何被分割到 componentDidMount 和 componentDidUpdate 中的，
  订阅逻辑又是如何被分割到 componentDidMount 和 componentWillUnmount 中的。
  而且 componentDidMount 中同时包含了两个不同功能的代码。
  
  hook允许我们按照功能去划分他们，React 将按照 effect 【声明的顺序】依次调用组件中的每一个 effect。
 ` function FriendStatusWithCounter(props) {
  const [count, setCount] = useState(0);
  // 改标题------
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });
  // 处理订阅----
  const [isOnline, setIsOnline] = useState(null);
  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });
  // ...
}
  `
  
  ### 为什么每次更新的时候都要运行 Effect？
  effect 的清除阶段在每次重新渲染时都会执行，而不是只在卸载组件的时候执行一次。
  它会在调用一个新的 effect 之前对前一个 effect 进行清理。
  `
    // Mount with { friend: { id: 100 } } props
    ChatAPI.subscribeToFriendStatus(100, handleStatusChange);     // 运行第一个 effect
    // Update with { friend: { id: 200 } } props
    ChatAPI.unsubscribeFromFriendStatus(100, handleStatusChange); // 清除上一个 effect
    ChatAPI.subscribeToFriendStatus(200, handleStatusChange);     // 运行下一个 effect
    // Update with { friend: { id: 300 } } props
    ChatAPI.unsubscribeFromFriendStatus(200, handleStatusChange); // 清除上一个 effect
  `
  实现了class组件中，componentDidUpdate去解决的状态更换问题
  
  ### 性能优化？
  在class中，我们可以在componentDidUpdate， getSnapshotBeforeUpdate等去处理
  在effect中，只要传递数组作为 useEffect 的第二个可选参数即可，只在这个参数变化时，执行里面的内容
  虽然 useEffect 会在浏览器绘制后延迟执行，但会保证在任何新的渲染前执行。React 将在组件更新前刷新上一轮渲染的 effect。

  
  `useEffect(() => {
      document.title = `You clicked ${count} times`;
    }, [count]); // 仅在 count 更改时更新
  `
  
  
  ## 自定义hook
  有时候我们会想要在组件之间重用一些状态逻辑。
  目前为止，有两种主流方案来解决这个问题：高阶组件和 render props。自定义 Hook 可以让你在不增加组件的情况下达到同样的目的。
  1. 封装个hook 
  `
  import React, { useState, useEffect } from 'react';
  function useFriendStatus(friendID) {
      const [isOnline, setIsOnline] = useState(null);
      function handleStatusChange(status) {
        setIsOnline(status.isOnline);
      }
      useEffect(() => {
        ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
        return () => {
          ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
        };
     });
     return isOnline;
  }
`
2. 调用它 ~
`
    function FriendStatus(props) {
      const isOnline = useFriendStatus(props.friend.id);
      if (isOnline === null) {
        return 'Loading...';
      }
      return isOnline ? 'Online' : 'Offline';
    }
`

3. 还是调用它
`
    function FriendListItem(props) {
      const isOnline = useFriendStatus(props.friend.id);
      return (
        <li style={{ color: isOnline ? 'green' : 'black' }}>
          {props.friend.name}
        </li>
      );
    }
`

其中2，3都可以调用1， Hook 的每次调用都有一个完全独立的 state —— 因此你可以在单个组件中多次调用同一个自定义 Hook。

### 自定义Hook的一些重点
1.  需要用use开头，不然的话由于无法判断某个函数是否包含对其内部 Hook 的调用，React 将无法自动检查你的 Hook 是否违反了Hook规则
2.  两个组件使用同样的HoOk不会共享state，每次调用Hook都是独立的state

 ## 低频Hook
 
 1. useContext ：
 接收一个 context 对象（React.createContext 的返回值）并返回该 context 的当前值。
 useContext(MyContext) 只是让你能够读取 context 的值以及订阅 context 的变化。
 你仍然需要在上层组件树中使用 <MyContext.Provider> 来为下层组件提供 context。
 `
 function Example() {
  const locale = useContext(LocaleContext);
  const theme = useContext(ThemeContext);
  // ...
}
`
 2. useReducer：
 
 `
 function Todos() {
  const [todos, dispatch] = useReducer(todosReducer);
  // ...
 `
 
 4. useLayoutEffect：
  其函数签名与 useEffect 相同，但它会在所有的 DOM 变更之后同步调用 effect。
  可以使用它来读取 DOM 布局并同步触发重渲染。在浏览器执行绘制之前，useLayoutEffect 内部的更新计划将被同步刷新。
 
  
  
## 常见小问题

*  有什么是Hook可以做Class不能做的？

*  Hook能覆盖Class的所有场景嘛？
    目前暂时还没有对应不常用的 getSnapshotBeforeUpdate，getDerivedStateFromError 和 componentDidCatch 生命周期的 Hook 等价写法。
    
*  生命周期方法要如何对应到 Hook？
    constructor：函数组件不需要构造函数。你可以通过调用 useState 来初始化 state。如果计算的代价比较昂贵，你可以传一个函数给 useState。
    getDerivedStateFromProps：改为 在渲染时 安排一次更新。
    shouldComponentUpdate：详见 React.memo.
    render：这是函数组件体本身。
    componentDidMount, componentDidUpdate, componentWillUnmount：useEffect Hook 可以表达所有这些(包括 不那么 常见 的场景)的组合。
    getSnapshotBeforeUpdate，componentDidCatch 以及 getDerivedStateFromError：目前还没有这些方法的 Hook 等价写法，但很快会被添加。

*  Hook 会替代 render props 和高阶组件吗？
    在大部分场景下，Hook 足够了，并且能够帮助减少嵌套。

*  我该如何使用 Hook 进行数据获取？

*  如何获取上一轮的 props 或 state？
 通过 ref 来手动实现：

    `function Counter() {
      const [count, setCount] = useState(0);
      const prevCountRef = useRef();
      useEffect(() => {
        prevCountRef.current = count;
      });
      const prevCount = prevCountRef.current;
      return <h1>Now: {count}, before: {prevCount}</h1>;
    }
    `  
    
 *  Hook 会因为在渲染时创建函数而变慢吗？
 不会且可以认为 Hook 的设计在某些方面更加高效：
    1. Hook 避免了 class 需要的额外开支，像是创建类实例和在构造函数中绑定事件处理器的成本。
    2. 符合语言习惯的代码在使用 Hook 时不需要很深的组件树嵌套。这个现象在使用高阶组件、render props、和 context 的代码库中非常普遍。组件树小了，React 的工作量也随之减少。
  传统上认为，在 React 中使用内联函数对性能的影响，与每次渲染都传递新的回调会如何破坏子组件的 shouldComponentUpdate 优化有关。
  Hook从三个方面解决了这个问题
    1.useCallback Hook
    `
    const memoizedCallback = useCallback(() => {
      doSomething(a, b);
    }, [a, b]);
    `
    2. useMemo Hook 使得控制具体子节点何时更新变得更容易，减少了对纯组件的需要。
    
    3. useReducer Hook 减少了对深层传递回调的依赖
   `
   const TodosDispatch = React.createContext(null);
    function TodosApp() {
      // 提示：`dispatch` 不会在重新渲染之间变化
      const [todos, dispatch] = useReducer(todosReducer);
      return (
        <TodosDispatch.Provider value={dispatch}>
          <DeepTree todos={todos} />
        </TodosDispatch.Provider>
      );
    }
    function DeepChild(props) {
      // 如果我们想要执行一个 action，我们可以从 context 中获取 dispatch。
      const dispatch = useContext(TodosDispatch);
      function handleClick() {
        dispatch({ type: 'add', text: 'hello' });
      }
      return (
        <button onClick={handleClick}>Add todo</button>
      );
    }
   `
  
  
  
  
  
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 