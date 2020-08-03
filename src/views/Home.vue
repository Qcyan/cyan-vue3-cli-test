<template>
  <div>
    <input class="new-todo" placeholder="想干的事" v-model="newTodo" @keyup.enter="addTodo" />
    <div v-show="todoList.length">
      <ul>
        <li v-for="(todo,index) in todoList" :key="index">{{todo}}</li>
      </ul>
      <p>共有条{{ListLength_W}}watch数据</p>
      <p>共有条{{ListLength_A}}真实数据</p>
    </div>
    <button v-show="showBtn" @click="clearAll">取消数据数量监听</button>

    <!-- <HelloWorld :title.sync="title"> 2.0写法-->
    <!-- 3.0写法 -->
    <HelloWorld v-model:title="title">
      <template v-slot>
        <div>hello hello</div>
      </template>
      <!-- <template v-slot:cyanslot> -->
      <template #cyanslot>
        <div>hello is vue3</div>
      </template>
    </HelloWorld>

    <!-- <div id="foot-container"></div> -->
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import {
  ref,
  reactive,
  toRefs,
  computed,
  watch,
  onMounted,
  onUpdated,
  onUnmounted,
} from "vue";
const { effect } = require("@vue/reactivity");

export default {
  name: "Home",
  components: {
    HelloWorld,
  },
  setup() {
    let count = ref(1);
    effect(() => {
      console.log("数据变化了,count是" + count.value);
    });
    let timer = setInterval(() => {
      count.value++;
      if (count.value >= 5) clearInterval(timer);
    }, 1000);

    const { todoState, addTodo, clearAll, ListLength_A } = todo();
    const title = ref("标题~~");

    onMounted(() => {
      console.log("当组挂载完成");
    });
    onUpdated(() => {
      console.log("数据发生更新");
    });
    onUnmounted(() => {
      console.log("组件将要卸载");
    });

    return {
      title,

      addTodo,
      ...toRefs(todoState),
      clearAll,
      ListLength_A,
    };
  },
};

function addNumber() {
  //随机数
  var str = "";
  for (var i = 0; i < 1; i += 1) {
    str += Math.floor(Math.random() * 10);
  }
  return str;
}

// todo逻辑
function todo() {
  const todoState = reactive({
    newTodo: "",
    todoList: [],
    ListLength_W: 0, //监听条数
    showBtn: false,
  });

  const stopWatch = watch(todoState.todoList, (val, oldVal) => {
    todoState.ListLength_W = val.length;
    if (val.length > 3) todoState.showBtn = true;
  });

  const ListLength_A = computed({
    get() {
      return todoState.todoList.length;
    },
    set(value) {
      todoState.ListLength_A = value;
    },
  });

  function addTodo() {
    var value = todoState.newTodo;
    if (!value) return;
    todoState.todoList.push({
      id: addNumber(),
      title: value,
    });
  }

  function clearAll() {
    stopWatch();
    alert("已取消记录数量");
  }

  return {
    addTodo,
    ListLength_A,
    todoState,
    clearAll,
  };
}
</script>
