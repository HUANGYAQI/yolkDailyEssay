# React的渲染：

  ## 从第一个React元素的诞生渲染开始说起
 
 1. 在JSX中，定义一个元素element，其中内容可以是静态、变量值、一些用户输入，都会进行转义，所有的内容，都被转义成了字符串，防止XSS的攻击.

   
    const element = <h1>Ylok Daily Essay,蛋黄日报</h1>
    const element = <h1>{ylok.name}</h1>
    const value = input.value
    const element = <h1>{value}</h1>
    
  
 2. 页面中存在一个根节点Root, 这个节点内的内容，都由ReactDom来管理，这个element，是React应用的最小砖头，想要将一个 React 元素【渲染】到根 DOM 节点中，就将他们一起传入ReactDOM.render().
   
   
    <Html>
    <div id="root"></div>
    
    <JSX>
    const element = <h1>Ylok Daily Essay,蛋黄日报</h1>
    ReactDOM.render(element, document.getElementById('root'))
    
    
 ## 每一个React元素，都独一无二
 
  1. 每一个React元素，都是不可变对象，我们更新UI的唯一方式，是创建一个新的对象，并传入ReactDom.render(). 官网的实例就是，每1秒执行一次tick,也就会创建一个新的React元素去更新UI.
  
    function tick() {
      const element = (
          <div>
            <h1>yolk Daily Essay!</h1>
            <h2>It is {new Date().toLocaleTimeString()}.</h2>
          </div>
        );
        ReactDOM.render(element, document.getElementById('root'))
      }

    setInterval(tick, 1000);
    
    
  
  
  
  ## 不得不提的Dom Diff算法，KEY与LIST不得不说的故事
  
  初始化：调用 React 的 render() 方法，会创建一棵由 React 元素组成的树。
  更新：  state 或 props 更新时，相同的 render() 方法会返回一棵不同的树。
  diff:  对这两棵树之间的差别来判断如何有效率的更新 UI 以保证当前 UI 与最新的树保持同步。
  
  如果使用【如何用最小步数从A树变成B树】，最优的算法，时间复杂度也是 O(n 3 )
  
  于是，大家被逼急了，提出了两个假设，史称yolkReact假设
  
  1. 两个不同类型的元素生成的树是不一样的
  2. 开发者可以通过Key Props来暗示哪些子元素是保持稳定的
  
  
  1. 对于一个节点，从根节点开始遍历。
  2. 对于不同类型的元素，销毁节点并重建，在React中，认为删除新建比移动开销要小
  3. 对于相同类型的元素，React 会保留 DOM 节点，仅比对及更新有改变的属性
  4. 对于相同类型的组件元素，组件实例不变，React 将更新该组件实例的 props 以跟最新的元素保持一致
  5. 对于同一个父节点下的多个相同标签（list）,基于Key，进行对比和修改
  
  其中：
  1. 使用前序遍历顺序遍历节点树
  2. 标签的不同`<div /><span />`不同，自定义标签`<A /><B />`不同， 标签的位置是相对父节点，原先是挂在A下，更新后在B下，那么这个也被销毁，再创建
  3. 例如`<image src='xxx'>`，更新前后只替换src 属性链接
  4. 调用该实例的 componentWillReceiveProps() 和 componentWillUpdate() 方法。render()更新节点
  5. 对于一个列表来说， 在尾部加入节点，开销较小
  
    <ul>
      <li>first</li>
      <li>second</li>
    </ul>

    <ul>
      <li>first</li>
      <li>second</li>
      <li>third</li>
    </ul>
    
   而如果是在头部添加节点，开销就会变得非常大
   
    <ul>
      <li>Duke</li>
      <li>Villanova</li>
    </ul>
    
    <ul>
      <li>Connecticut</li>
      <li>Duke</li>
      <li>Villanova</li>
    </ul>
    
  所以引入了key,React通过老树和新树上的key,来判断差异。
  
  6. 这个key,不用在全局唯一，而是在这个列表中唯一。
  7. 其中有一些特殊情况，例如，使用数组的下标作为key，这个策略在元素不进行重新排序时比较合适，但一旦有顺序修改，diff 就会变得慢。
  8. 当基于下标的组件进行重新排序时，组件 state 可能会遇到一些问题。比如反序时，index顺序不变，可能出现元素不按期改变更新
  9. 不稳定的 key（比如通过 Math.random() 生成的）会导致许多组件实例和 DOM 节点被不必要地重新创建，这可能导致性能下降和子组件中的状态丢失。
  
    
  
  
  
  
  
  
  
    
    
  
   
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
 