<template>
  <form @submit.prevent="addNewTodo">
    <input class="inputForm" v-model="newTodo" name="new_todo">
    <button class="addButton">追加</button>
  </form>
  <p class="a">{{ fetched_tasks }}</p>

  <!--以下のv-bindやidの概念について、しっかり理解したい-->
  <ul>
    <li class="tasks" v-for="task in fetched_tasks" v-bind:key="task.id">
      <input type="checkbox" id="checkbox">
      <p class="taskNumber">{{task.id}}.</p>
      <h3 class="taskTitle">{{task.content}}</h3>
    </li>
  </ul>
  <delete class="deleteButton" @click="deleteTask">
    <button>完了したタスクを除去</button>
  </delete>
</template>

<script>
import { ref } from 'vue';
// import { reactive } from "@vue/composition-api";

export default {
  name: 'Form',
  props: {
    ic:String
  },

  setup(){
    // const state = reactive({
    //   fetched_tasks:[]
    // })
    // return {
    //   state,
    // };
    const newTodo = ref('');
    let tasks = ref([]);
    let count = 1;
    let STORAGE_KEY = 'todos-vuejs-demo';

    let fetched_tasks = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]')
    console.log("テスト");
    console.log(fetched_tasks);

    function addNewTodo(){
      tasks.value.push({
        id: count,
        done: false,
        content: newTodo.value,
      });
      newTodo.value="";
      localStorage.setItem(STORAGE_KEY, JSON.stringify(tasks));
      countAdding();
    }

    function countAdding(){
      count+=1;
      console.log(count);
    }

    function deleteTask(){
      if(confirm("全タスクを削除します。よろしいですか？")){
        console.log("タスクを全て削除します。")
        this.tasks.splice(0, this.tasks.length);
        return;
      }
    }
    return{
      newTodo,
      fetched_tasks,
      addNewTodo,
      countAdding,
      deleteTask,
    };
  }
}
</script>

<style>
.a{
  color:white;
}
.tasks{
  padding-left:30px;
}

form{
  text-align: center;
}
.deleteButton{
  display:block;
  width:150px;
  margin:0 auto;
}
.taskNumber {
  display:inline-block;
}
.taskTitle {
  display:inline-block;
}
.inputForm{
  display:inline-block;
  margin-bottom:10px;
}
.addButton{
  display:inline-block;
  margin-left:10px;
  margin-bottom:10px;
}
</style>