<template>
  <div class="todolist">
    <button class="add" v-on:click="addNewTodo">Add</button>
    <div v-show="isAdd">
      <label>title: </label>
      <input type="text" name="title" v-model="title"/>
      <label>content: </label>
      <textarea name="content" v-model="content"/>
      <button v-on:click="submit">Submit</button>
    </div>
    <!---
      * v-for
      * 1. 用于循环，即一个迭代器
      * 2. 循环的对象可以是数组，对象或者一个数值（如 v-for="n in 10", n 将依次取值为 1，2，3， ...，10）
    -->
    <todoitem v-for="item in todolist" v-bind:key="item.id" v-bind:item="item" />
  </div>
</template>

<script>
import { EventBus } from '../event-bus.js'
import todoitem from '../components/TodoItem' // 调用子组件
import TodoStatus from '../todoStatus'

export default {
  name: 'TodoList', // 组件名
  components: { todoitem }, // 注册子组件
  created () { // 生命周期！实例（在这里指的是该 TodoList 组件）被创建之后执行
    EventBus.$on('moveItem', itemId => {
      this.moveItem(itemId) // this 指向调用它的 vue 实例（该 TodoList 组件）
    })
  },
  data () {
    return {
      todolist: [
        {
          id: 1,
          title: 'a todo item1',
          content: 'want to do',
          status: 'TODO',
          text: TodoStatus.TODO.text
        },
        {
          id: 2,
          title: 'a todo item2',
          content: 'want to do',
          status: 'TODO',
          text: TodoStatus.TODO.text
        },
        {
          id: 3,
          title: 'a todo item3',
          content: 'want to do',
          status: 'TODO',
          text: TodoStatus.TODO.text
        },
        {
          id: 4,
          title: 'a todo item4',
          content: 'want to do',
          status: 'FINISHED',
          text: TodoStatus.TODO.text
        }
      ],
      nextTodoId: 5,
      isAdd: false
    }
  },
  methods: {
    addNewTodo: function () {
      this.isAdd = true
    },
    submit: function () {
      this.todolist.push({
        id: this.nextTodoId,
        title: this.title + this.nextTodoId, // 获取输入的 title
        content: this.content, // 获取输入的 content
        status: 'TODO',
        text: TodoStatus.TODO.text
      })

      this.nextTodoId++
      // 清除 input 输入
      this.title = ''
      this.content = ''
    },
    moveItem: function (itemId) {
      this.todolist = this.todolist.filter(item => item.id !== itemId)
    }
  }
}
</script>
