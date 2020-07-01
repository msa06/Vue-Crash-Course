<template>
  <div id="app">
    
    <AddTodo v-on:add-todo="addItems"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteItems"/>
  </div>
</template>

<script>
import Todos from '../components/Todos';

import AddTodo from '../components/AddTodo';
import axios from 'axios';

export default {
  name: 'Home',
  components: {
    Todos,
    AddTodo
    
  },
  data(){
    return {
      todos:[],
      url:'https://jsonplaceholder.typicode.com/todos'
    }
  },
  methods:{
    deleteItems(id){
      axios.delete(this.url+`/${id}`)
      .then(() => this.todos = this.todos.filter(todo => todo.id !== id))
      .catch(err => console.log(err))
      
    },
    addItems(newTodo){
      const {title,completed} = newTodo;
      axios.post(this.url,{
        title,completed
      }).then(res => this.todos = [...this.todos,res.data])
      .catch(err => console.log(err))
      
    }
    
  },
  created(){
    axios.get(this.url+'?_limit=5')
    .then(res=> this.todos = res.data)
    .catch(err => console.log(err));
  }
}
</script>

<style>
*{
  box-sizing: border-box;
  margin:0;
  padding:0;
}
body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  line-height: 1.4;
}

.btn{
  display: inline-block;
  border:none;
  background: #555;
  color:#fff;
  padding:7px 20px;
  cursor:pointer;
}

.btn:hover{
  background: #666;
}
</style>
