<template>
  <div class="todo-item">
   <div class="header">
     <h1 class="title">{{item.title}}</h1>
     <span class="todo" v-bind:class="{active: item.status === 0}">{{item.status}}</span>
   </div>
   <div class="content">{{item.content}}</div>
   <div class="footer">
      <input type="radio" class="action finished" value="finished" v-on:click="onSelect"/>完成
      <button class="action" v-on:click="onDelete(item.id)">删除</button>
      <button class="action">编辑</button>
   </div>
  </div>
</template>

<script>
import { EventBus } from '../event-bus.js'

export default {
  name: 'TodoItem',
  props: ['item'], // 接受父组件传递的数据
  data () {
    return ({
      isEditable: true
    })
  },
  methods: {
    onDelete: function (itemId) {
      EventBus.$emit('moveItem', itemId)
    },
    onEditor: function () {
      this.isEditable = true
    },
    onSelect: function () {
      console.log('被選中啦')
    }
  }
}
</script>

<style>
.todo-item {
  margin-left: 100px;
  max-width: 400px;
  padding: 10px;
  border: 1px solid #666;
}

.deleted {
  visibility: hidden;
}

.header {
  display: inline-block;
  text-align: left;
  position: relative;
  width: 100%;
  padding: 6px 10px;
  box-sizing: border-box;
  border-bottom: 1px dotted #666;
}

.title {
  margin: 0;
  display: inline-block;
  font-size: 14px;

}

.todo {
  display: inline-block;
  background-color: #41b883;
  color: #35495e;
  border-radius: 10px;
  width: 60px;
  height: 22px;
  text-align: center;
  float: right;
}

.content {
  width: 100%;
  min-height: 80px;
}

.action {
  border: 0;
  outline: 0;
  width: 80px;
  height: 30px;
  font-size: 16px;
  color: #444;
  cursor: pointer;
}

.action + .action {
  margin-left: 30px;
}

.finished {
  width: 20px;
  height: 20px;
  line-height: 30px;
  vertical-align: middle;
}
</style>
