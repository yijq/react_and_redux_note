# 从Flux到Redux

## Flux
- 单向数据流
- 步骤:
  - 创建一个全局的dispatcher
  - 创建动作类型actionType,为动作名称字符串
  - 创建动作类型的生成器action,每个动作dispacth一个含有type和动作事件数据的对象
  - 创建对应的store,用于存储数据,根据对应的动作类型更改状态,返回对应的状态,监听事件,移除事件
  - 在组件中使用需要的action来修改store,引入需要的store用来监听改变
- 优点:
  -  单向数据流,容易控制
- 缺点:
  - store之间的依赖关系
  - 难以进行服务端渲染
  - store混杂了逻辑和状态

## Redux
- Single Souce of Truth(唯一数据源)
- State is read-only(保持状态只读)
- Change are made with pure function(数据改变只能通过纯函数完成)
- 步骤:
  - 创建动作类型actionType,为动作名称字符串
  - 创建动作生成器action,返回一个含有type和动作事件数据的对象
  - 创建动作处理函数reducer,为纯函数,接受state和action两个参数,返回新的state
  - 创建store,用于储存state,createStore(reducer,initialState,applyMiddleWare)
  - 在组件中使用store.dispatch(action)来触发一个动作改变store,使用store.getState()来得到当前的store,使用store.subscribe()来监听store的改变,unsubscribe()来移除监听

- 扩展:
  - 容器组件和傻瓜组件
  - 使用原生属性context优化
    ```
      //父组件声明提供context
      class Parent extends Compoent {
        getChildContext() {
          return {
            store: tis.props.store
          }
        }

        render() {
          return this.props.children;
        }
      }

      Provider.childContextTypes = {
        store: PropTypes.object
      };

      //子组件声明使用父组件的context
      class Child extends Component {
        constructor(props,context) {
            super(props,context);
            //super(...arguments);
        }
      }

      Child.contextType = {
        store: PropTypes.object
      }
    ```
  - react-redux:
    ```
    Provider

    function mapStateToProps(state, ownProps) {
      return {
        value: state[ownProps.caption]
      }
    }

    function mapDispatchToProps(dispatch, ownProps) {
      return {
        onIncrement: () => {
          dispatch(Actions.increment(ownProps.caption));
        },
        onDecrement: () => {
          dispatch(Actions.decrement(ownProps.caption));
        }
      }
    }

    export default connect(mapStateToProps, mapDispatchToProps)(Counter);
    ```
