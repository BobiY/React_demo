
     首先你需要记住，React只是 MVC 框架中的 V 层框架，它不具备 M 和 C 的功能，所以只是负责页面展示的。
     React 能做的是  1.展示你的 App 的页面。
                    2.在任意时刻你的数据改变时都能及时而快速的更新页面
                    3.根据你的数据变化自动的识别页面中哪块书需要更新的，哪块是不需要更新的
                    4.React 是基于可复用的组件开发的，你只需要考虑组件是怎么写的就可以了。由于这个特性，React 组件的封装性很强，这就给测试，复用和后期维护提供了更大的便利。
                    5.React 是facebook 的一群疯狂的工程师开发出来的，有很多颠覆常规的用法，下面我们就从 Hello World 开始学习.
     当一个组件被挂载以后会拥有以下四个 API ：

                    1.this.setState(); 作用是修改 State 会和老的 State 就行合并，并且触发 render 进行组件更新。
                    2.this.replaceState();作用跟 setState类似，但是他是直接删除老的 State ，设置新的 State ，也会触发 render 进行组件更新。
                    3.this.forceUpdate();作用是触发组件更新（在redux中有应用），他的更新是跳过 shouldComponentUpdate 直接更新
                    4.this.isMounted();作用是判断组件是否被挂载，如果是返回 true 否则返回 false；
                    这四个方法中，setState()用的最多，其他的三种应用的少。

      组件的返回值：
                    1.组件必须有 render 方法，并且必须有返回值。
                    2.render 方法的返回值必须是一个react 节点或者react组件，组件或者节点可以包含任意多个子节点或者组件。
                    3.在返回的dom结构超过一行时，必须用一个括号将返回值包起来。
                    4.在返回的dom结构超过一行时，只能有一个最外层的节点。

      获取组件实例的方法：

                    当一个组件被render后，我们有两种方法来获取组件实例及内部属性(this.props 和 this.state)：
                        1.通过 this 关键字，当组件被挂载以后，其内部的 this 就指向了组件的实例。
                        2.ReactDOM.render()方法会返回最外层组件的实例。

      React组件的生命周期：

                    React的生命周期主要分三个阶段：
                         1.挂载阶段：
                               这个阶段执行的方法依次是
                                   getDefaultProps();这个方法是获取初始化的this.props的值，并且它只能在ES5语法创建组中使用，在ES6中使用 MyComponent.defaultProps = {};来初始化。
                                   getInitalState();这是获取初始化的 this.state的值，只能在ES5语法创建组件中使用，在ES6中直接使用this.state = {};来初始化；
                                   componentWillMount();组件将要挂载时调用，这里是你更改 State 的最后的机会
                                   componentDidMount();组件挂载完毕时的方法，这里你可以发送异步请求获取初始数据，使用 jquery；
                         2.更新阶段：
                               这个阶段执行方法依次是：
                                   componentWillReceiveProps(nextProps);这个方法是组件接收到新的 Props 但还未更新时，这里你可以对比新老 Props ，使用 this.setState() 方法进行状态改变。
                                   shouldComponentUpdate(nextProps,nextState);这个方法是判断组件是否需要更新，常常用来优化 React的性能，判断新老 Props 和 State 以判断组件是否需要更新，这个方法必须有返回值，返回 true表示组件需要更新，返回 false 表示组件不进行更新；
                                   componentWillUpdate();这个方法和 componentWillMount() 类似，但是区别也很明显，在这个方法里不能修改 State ，如果你强行去修改，就会导致组件循环渲染，也就相当于你写了一个死循环。
                                   componentDidUpdate();这个方法是组件更新完毕时调用的，这里你可以发送异步请求获取更新数据，使用 jquery；
                          3.写在阶段：
                               这个阶段只有一个函数：
                                  componentWillUnmount();这个方法是在组件卸载之前调用的，你可以做一些提示类的信息。

                               这里做一个拓展，就是什么时候组件会被卸载，这里介绍一种手动卸载的方法

                                   ReactDOM.unmountComponentAtNode(document.body);参数是DOM节点,这个方法的作用就是在DOM节点中卸载 React 组件，开发中较少使用。

                               还有一种情况是路由切换时，React 也会将对应的组件卸载。

        获取子组件和子节点的方法：

                     1.如果一个组建包含一些子组件或者一些子节点，可以通过 this.props.children来获取，但对于嵌套层次很深的元素，外层组件可通过 this.props.children[1].props.children....,来获取更深层次的子组件。但是这样子的获取方式不推荐使用。
                     2.这里需要注意的是子组件指的是组件实例包围起来的元素，而非组件里面的元素。

                     由于React的子组件类型可能是 数组 对象 null undefind ，所以我们不能用常规的方法去操作，因为获取的是哪种类型的数据我们并不清楚，所以 React提供了几种操作子组件的方法：

                         1.React.Children.map(this.props.children,function(children){});
                             这个方法的使用方式类似于 Array.map(),最后的返回值是一个数组，数组里里面是每个子节点的具体信息。
                             这里还有一点要记住，React是会自动的将数组展开显示的。
                         2.React.Children.forEach(this.props.children,function(children){});
                              这个方法与map一样，只是没有返回值。
                         3.React.Children.count(this.props.children);
                              获取当前子组件或者节点的数量。与map 和 forEach 循环次数相等。
                         4.React.Children.only(this.props.children);
                              返回唯一的子元素，否则报错。
                         5.React.Children.toArray(this.props.children);
                              返回一个子组件或者节点组成的数组，你可以操作子元素，在render中，map方法的返回值和这个方法的返回值在大多是情况下是相同的。
       获取DOM节点的方法:

                   1.ref
                       ref属性使得我们获取了对某一个React节点或某一个子组件的引用，这个在你需要直接操作DOM时非常有用。

                      ref的使用方式也很简单
                      给你想引用的的子元素或组件添加ref属性，然后在本组件中通过this.refs.value（你所设置的属性名）即可引用
                      ref还有一种函数式的写法，你可以将他的引用赋值给一个变量，通过这个变量就可以拿到对应的DOM元素，记住，这里所说的DOM元素是真实的DOM元素，而非虚拟DOM。
                   2.React.findDOMNode()

                      这个方法的参数是ref获得的元素的引用，这样拿到的元素，你可以当做由原生js Dom API 获得的元素来使用。

        babel：https://zhangwang1990.gitbooks.io/reactenlightenment/content/React%E5%92%8CBabel%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.html
        Form 组件：

                
