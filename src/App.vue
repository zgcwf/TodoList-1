<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <!-- 2.props：将父组件函数传递给子组件 
        <MyHeader :addTodo="addTodo"></MyHeader>
        -->
        <!-- 使用自定义事件 -->
        <MyHeader @addTodo="addTodo"></MyHeader>

        <!-- 2.props：将父组件函数传递给子组件 
          <MyList
          :todos="todos"
          :checkTodo="checkTodo"
          :deleteTodo="deleteTodo"
        ></MyList> -->
        <MyList :todos="todos"> </MyList>

        <!--  2.props：将父组件函数传递给子组件 
          <MyFooter
          :todos="todos"
          :checkAllTodo="checkAllTodo"
          :clearAllTodo="clearAllTodo"
        ></MyFooter> -->
        <!-- 使用自定义事件 -->
        <MyFooter
          :todos="todos"
          @checkAllTodo="checkAllTodo"
          @clearAllTodo="clearAllTodo"
        ></MyFooter>
      </div>
    </div>
  </div>
</template>

<script>
import MyHeader from "./components/MyHeader.vue";
import MyFooter from "./components/MyFooter.vue";
import MyList from "./components/MyList.vue";

export default {
  name: "App",
  components: {
    //注册组件
    MyHeader,
    MyFooter,
    MyList,
  },
  data() {
    return {
      //由于todos是MyHeader组件和MyFooter组件都在使用，所以放在App中（状态提升）
      // todos: [
      //   { id: "001", title: "抽烟", done: true },
      //   { id: "002", title: "喝酒", done: false },
      //   { id: "003", title: "开车", done: true },
      // ],
      todos: JSON.parse(localStorage.getItem("todos")) || [],
    };
  },
  methods: {
    //添加一个todo
    addTodo(todoObj) {
      this.todos.unshift(todoObj);
      //  1.父组件定义函数,无论是自定义事件还是props，还是全局事件总线子传父这都是第一步
    },
    //勾选or取消勾选一个todo
    checkTodo(id) {
      this.todos.forEach((todo) => {
        if (todo.id === id) {
          todo.done = !todo.done;
        }
      });
    },
    // 删除一个todo
    deleteTodo(id) {
      this.todos = this.todos.filter((todo) => {
        // 过滤出todo.id不等于id的属性返回
        return todo.id !== id;
      });
    },
    //全选or取消全选
    checkAllTodo(done) {
      this.todos.forEach((todo) => {
        todo.done = done;
      });
    },
    //清除所有已经完成的todo
    clearAllTodo() {
      this.todos = this.todos.filter((todo) => {
        return todo.done === false;
      });
    },
  },
  watch: {
    todos: {
      deep: true,
      // 开启深度监视
      // 当todos中的数据发生变化时，handler自动调用
      handler(value) {
        localStorage.setItem("todos", JSON.stringify(value));
        // console.log("@", value);
      },
    },
  },
  // 4.全局事件总线接收数据并回调函数
  mounted() {
    this.$bus.$on("checkTodo", this.checkTodo);
    this.$bus.$on("deleteTodo", this.deleteTodo);
  },
  beforeDestroy() {
    this.$bus$off("checkTodo");
    this.$bus$off("deleteTodo");
  },
};
</script>

<style>
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>