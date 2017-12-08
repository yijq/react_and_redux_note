# 设计高质量的react组件

## 划分组件边界的原则
- 分而治之
- 高内聚,低耦合

## react组件的数据
```
//父类传值
 <Counter caption="Second" initValue={10} />

//子类中读取值
class Counter extends React.Component{
    constructor(props) {
        super(props)
    }
}

//propsTypes检查
Counter.propTypes = {
    caption: PropTypes.string.isRequired,
    initValue: PropTypes.number
}

//默认props
Counter.defaultProps = {
  initValue: 0
};
```
- props: 组件的对外接口(子组件内不应该修改props的值)
    - props支持任何一种JavaScript支持的数据类型
    - style需要两层花括号,第一层用于包裹对象,第二层为对象语法
    - 子类读取值时需提升props的状态
    - propsTypes检查,babel-react-optimize可以在编译时去掉propsTypes检查

```
//初始化state
class Counter extends React.Component{
    constructor(props) {
        super(props)
        this.state = {
            count: props.initValue || 0
        }
    }
}

//读取与更新state
onClickIncrementButton() {
    this.setState({count: this.state.count + 1});
  }

```
- state: 组件的内部状态
    - state必须是对象
    - 读取与更新state使用this.setState(updater[, callback])
    - this.setState((prevState, props) => {
  return {counter: prevState.counter + props.step};
});


## react组件的生命周期
- 装载过程
    - 组件第一次被渲染时调用
    - constructor(props){}
        - 初始化state
        - 绑定成员函数的this环境
    - getInitalState和getDefaultProps(ES6中已经不适用)
    - componentWillMount(){}
    - render(){}
    - componentDidMount(){}
- 更新过程
    - 当porps或state被修改时调用
    - componentWillReceiveProps(nextProps) {}
        - 父组件中调用render()时调用,this.setState不会触发

    - shouldComponentUpdate(nextProps,nextState) {}
        - 返回一个bool值,决定是否渲染
    - componentWillUpdate(nextProps,nextState) {}
    - render()
    - componentDidUpdate(prevProps, prevState) {}
- 卸载过程
    - componentWillUnmount() {}
    - componentDidCatch(error, info) {}

- tips:
    - 组件可通过this.forceUpdate强行引发一次重新绘制
