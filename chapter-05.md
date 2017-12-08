## React组件的性能优化

### 单个React组件的性能优化
- 原理:在shouldComponentUpdate(nextProps,nextState){}中来判断是否需要更新,同时也要避免给子组件传匿名对象或函数
- 可以使用react-redux中的connect的特性,将函数写在map函数中

### 多个React组件的性能优化
- shouldComponentUpdate的使用
- key: 必须唯一,必须不变,不能用数组索引来作为key

### 利用reselect提高数据选取的性能
- reselect可以用来比较是否改变来提高性能
  ```
  import {createSelector} from 'reselect';
  import {FilterTypes} from '../constants.js';

  const getFilter = (state) => state.filter;
  const getTodos = (state) => state.todos;

  export const selectVisibleTodos = createSelector(
    [getFilter, getTodos],
    (filter, todos) => {
      switch (filter) {
        case FilterTypes.ALL:
          return todos;
        case FilterTypes.COMPLETED:
          return todos.filter(item => item.completed);
        case FilterTypes.UNCOMPLETED:
          return todos.filter(item => !item.completed);
        default:
          throw new Error('unsupported filter');
      }
    }
  );

  ```
- store的状态设计尽量范式化,类似关系型数据库
