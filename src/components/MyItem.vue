<template>
  <li>
    <label>
      <input
        type="checkbox"
        :checked="todo.done"
        @change="handleCheck(todo.id)"
      />
      <!-- 如下代码也能实现功能，甚至不用添加函数事件，但是不太推荐，因为有点违反原则，因为修改了props -->
      <!-- <input type="checkbox" v-model="todo.done"/> -->
      <span>{{ todo.title }}</span>
    </label>
    <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
  </li>
</template>

<script>
export default {
  name: "MyItem",
  /*3.props：子组件接收父组件传递的函数
   props: ["todo", "checkTodo", "deleteTodo"],
   */
  props: ["todo"],

  methods: {
    //勾选or取消勾选
    handleCheck(id) {
      //  console.log(id);
      //数据在哪里，操作数据的方法就在哪里,所以去App.vue中操作
      //通知App组件将对应的todo对象的done值取反
      /*  4.props：调用这个函数勾选
        this.checkTodo(id);
      */

      // 3.全局事件总线传递数据
      this.$bus.$emit("checkTodo", id);
    },
    //删除
    handleDelete(id) {
      if (confirm("确定删除吗？")) {
        //通知App组件将对应的todo对象删除
        /*  4.props：调用这个函数删除
         this.deleteTodo(id);
         */

        // 3.全局事件总线传递数据
        this.$bus.$emit("deleteTodo", id);
      }
    },
  },
};
</script>

<style scoped>
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}
li:hover {
  background-color: #ddd;
}
li:hover button {
  display: block;
}
</style>