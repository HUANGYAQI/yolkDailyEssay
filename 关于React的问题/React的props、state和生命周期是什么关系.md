# Props和State的孽缘

## 先来说说函数组件和类组件

  1. 说到底React也是JS，那么定义一个组件最方便的方式就是一个JS函数：
  
  ```js
   function build (props) {
      return (<div>{props.value}</div>)
   }

  ```
   像这样，接受props传参，返回一个React元素，这类组件，我们就叫他【函数组件】.
   
   2. 使用ES6的Class去定义组件：
   
   ```class build extends React.component {
          render() {
            return <div>yolk, {this.props.name}</div>;
          }
       }
   ```
   这种就是我们说的class组件.
   
   * 以上两种组件是等效的
   
   
   ## 组件的渲染
   
   一个React元素，可以是一个DOM元素，也可以是一个组件元素.当时一个组件元素的时候，会将所接受的属性或者子组件传递给组件，这个现象被称之为‘props’.
   
   ```jsx
       function build (props) {
          return (<div>{props.value}</div>)
       }
   
      const element = <build value='yolkDailyEssay' />

      ReactDOM.render(
         element,
         document.getElementById('root')
      )

  ```
   例如上面的代码，就应该会渲染出5.
   1. 调用ReactDOM.render(),将`<build value='yolkDailyEssay' />`传入React中。
   2. React调用Build组件，并传入{value: 5}
   3. Build 接受‘props’,并返回`<div>{props.value}</div>`
   4. ReactDOM将节点更新。
   
   
   ## 有状态组件 和 无状态组件 && 受控组件 和 非受控组件
   1. 无状态的函数创建的组件是无状态组件，它是一种只负责展示的纯组件，它的特点是不需要管理状态state，数据直接通过props传入，这也符合 React 单向数据流的思想
   `
        function HelloComponent(props) {
            return <div>Hello {props.name}</div>
        }
        ReactDOM.render(<HelloComponent name="marlon"/>, mountNode) 
   `
   
   2. 在无状态组件的基础上，如果组件内部包含状态（state）且状态随着事件或者外部的消息而发生改变的时候，这就构成了有状态组件（Stateful Component）。
   
   基本来说，无状态组件使用props来存储数据，而有状态组件使用state来存储数据。
   
   3. 受控组件，在HTML中，标签<input>、<textarea>、<select>的值的改变通常是根据用户输入进行更新。在React中，可变状态通常保存在组件的状态属性中，并且只能使用 setState() 更新，
      以这种由React控制的输入表单元素而改变其值的方式，称为：“受控组件”。  
      
   4. 不受控组件，表单数据由DOM本身处理。即不受setState()的控制，组件没有设置value （checked）时，就可以称之为非受控组件，
      组件设置了defaultValue (defaultChecked)来表示组件的默认值，此时它只会被渲染一次，后续的渲染不起作用。
   
    
   ## 事件处理
   分别通过箭头函数和 Function.prototype.bind 来实现。
  `<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
  <button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>`
  
   
   
   ## 绝代双骄props
   
   我们说到一个React元素，可以是一个DOM元素，也可以是一个组件元素.当时一个组件元素的时候，会将所接受的属性或者子组件传递给组件，这个现象被称之为‘props’.
   了解了什么是props,也就可以体会到一个重要的性质，无论你用函数组件还是使用class组件，props都是【只读】的，决不能修改自身的props。
   无论你是这样的纯函数,多次调用保持相同结果
   
   
   ```jsx
   function add (x, y) {
        return x + y;
   }
  ```

   还是这样修改了自己的入参
   ```jsx
   function add (account, amount) {
        return account.all -= amount;
   }
  ```

  你都应该让每个React组件像‘纯函数’那样去保护他们的Props不被修改。
  
  ## 末路狂花之state
  
  我们知道了，要更改React的UI，只有通过ReactDOM.render去更新这个节点。
  就像下面这样，每秒钟更新一下蛋黄的狗毛数量，我们就得去调用render多少次。
  
  ```jsx
   function Yolk (props) {
      return (
        <div>
          <h1>这里有蛋黄的狗毛一共：</h1>
          <h2>{props.number}根</h2>
        </div>
      );
    }

   function time () {
      ReactDOM.render(
        <Yolk number={new Date().toTimeString()}>
      ),
     document.getElementById('root')
   } 
  setInterval(time, 1000);
  
  ```

 如何让他自己去render呢？
 
 引入了一个‘state’,类似于props，但是state私有，完全受控与组件，可以理解为组件内部的存储空间。
    ```jsx
         class Yolk extends React.Component {
          constructor(props) {
              this.state = {
                number: new Date().toTimeString(),
              }
          }
          const {number} = this.state;
          render() {
                return (
                  <div>
                    <h1>这里有蛋黄的狗毛一共：</h1>
                    <h2>{number}根</h2>
                  </div>
                );
          }
        }
        ReactDOM.render(
          <Yolk />,
          document.getElementById('root')
        );
    ```
    
   我们做了什么呢？
   1. 把函数组件 -> class组件
   2. props.number -> state.number
   3. 给class组件一个constructor，并初始化state中number的初始值
    
   这样，每一次生成了新的state,就会触发React去重新判断是否需要更新，和如何更新。
   
  
   ## 生死有命富贵在天那么React的生命周期？
   
   在我们的class组件中
   ```jsx
        class yolk extends React.Component {
          constructor(props) {
            super(props);
            this.state = {date: new Date().toTimeString()};
          }
        
          componentDidMount() {
          }
         ...
          componentWillUnmount() {
          }
        
          render() {
            return (
              <div>
                <h2>{this.state.date}.</h2>
              </div>
            );
          }
        }

   ```
   
   总结就是：
   1. 当组件挂载时：constructor() -> static getDerivedStateFromProps() -> render() -> componentDidMount()
   2. 当组件更新时（当组件的 props 或 state 发生变化时会触发更新）：
      static getDerivedStateFromProps() -> shouldComponentUpdate() -> render() -> getSnapshotBeforeUpdate() -> componentDidUpdate()
   3. 组件卸载时：componentWillUnmount()
   4. 错误处理时： static getDerivedStateFromError() -> componentDidCatch()

   * 其中：
   * getSnapshotBeforeUpdate(prevProps, prevState)： 在最近一次渲染输出（提交到 DOM 节点）之前调用。它使得组件能在发生更改之前从 DOM 中捕获一些信息（例如，滚动位置）。此生命周期的任何返回值将作为参数传递给 componentDidUpdate()
 
   
   ## 除了上面主动会去调用的生命周期，你只可以在组件中使用的 setState() 和 forceUpdate()
   
   `setState(updater, [callback])`
   
   1. setState() 将对组件 state 的更改排入队列，并通知 React 需要使用更新后的 state 重新渲染此组件及其子组件。
   2. setState() 是浅合并
   3. setState() 并不总是立即更新组件。为了更好的性能，他会批量更新延迟调用，setState() 的第二个参数为可选的回调函数，它将在 setState 完成合并并重新渲染组件后执行。
   
   同样，对于setState 也存在两种调用形式：
   
   1. `this.setState({comment: 'Hello'});`
   
   2.  通过传入一个函数，而不是一个对象，这个函数用上一个 state 作为第一个参数，将此次更新被应用时的 props 做为第二个参数
      
      `this.setState((state, props) => ({
          counter: state.counter + props.increment
      }));`
      
   对于React来说，除非shouldComponentUpdate()返回false,不然每次state的改变，都会引起重新渲染。
   
   
   `component.forceUpdate(callback)`
   1. 默认情况下，当组件的 state 或 props 发生变化时，组件将重新渲染。
   2. 如果 render() 方法依赖于其他数据，则可以调用 forceUpdate() 强制让组件重新渲染。
   3. 要避免使用
      
   
   ## 数据是自上往下单向流动的
   
   无论是什么组件，都无法知道下面的组件是有状态还是无状态的组件，也不关心他们是函数组件，还是class组件，
   可以将本组件的state作为props向下传递，这通常会被叫做“自上而下”或是“单向”的数据流。
   任何的 state 总是所属于特定的组件，而且从该 state 派生的任何数据或 UI 只能影响树中“低于”它们的组件。
   
    
   
   
   
   
   


   

   
   
   
   
   
  
  
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
