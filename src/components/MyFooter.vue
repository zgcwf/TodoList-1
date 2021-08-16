<template>
  <div class="todo-footer" v-show="total">
    <label>
      <!-- <input type="checkbox" :checked="isAll" @change="checkAll" /> -->
      <!-- 可以简写为下方这句代码，isAll是自己计算出来的，不是props属性 -->
      <input type="checkbox" v-model="isAll" />
    </label>
    <span>
      <span>已完成{{ doneTotal }}</span> / 全部{{ total }}
    </span>
    <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
  </div>
</template>

<script>
export default {
  name: "MyFooter",
  props: ["todos", "checkAllTodo", "clearAllTodo"],

  computed: {
    total() {
      return this.todos.length;
    },
    doneTotal() {
      let i = 0;
      this.todos.forEach((p) => {
        if (p.done) i++;
      });
      return i;
      //   return this.todos.reduce((pre, todo) => pre + (todo.done ? 1 : 0), 0);
    },
    isAll: {
      //全选框是否勾选
      get() {
        return this.doneTotal === this.total && this.total > 0;
      },
      //isAll被修改时set被调用
      set(value) {
        this.checkAllTodo(value);
      },
    },
  },

  methods: {
    /* 用v-model就可以省略这句代码了
    checkAll(e) {
      //   console.log(e.target.checked);   打印出点击事件的值
      this.checkAllTodo(e.target.checked);
    },
    */
    //清空所有已完成
    clearAll() {
      this.clearAllTodo();
    },
  },
};
</script>

<style scoped>
/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>