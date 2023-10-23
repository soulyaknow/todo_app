<template>
    <div class="container">
      <h3>Task</h3>
      <div class="actions">
        <form @submit="addTask">
          <div id="input-button">
            <input type="text" placeholder="Add Task" v-model="task" required id="task">
            <button type="submit" id="add">Add<i class="fa fa-plus" aria-hidden="true"></i></button>
          </div>
          <input type="text" placeholder="search task" id="search" v-model="searchQuery">
        </form>
      </div>
      <div class="tasks">
        <div class="task-items" v-for="todo in filteredTodos" :key="todo.id">
          <p :class="{ done: todo.status }">{{ todo.details }}</p>
          <button class="done-btn" @click="doneTask(todo.id)"><i class="fa fa-check" aria-hidden="true"></i></button>
          <button class="remove-btn" @click="removeTask(todo.id)"><i class="fa fa-times" aria-hidden="true"></i></button>
        </div>
          <button class="clear-btn" v-if="hasTasks" @click="clearAllTasks">Clear Task</button>
      </div>
    </div>
</template>

<script>
import { v4 as uuidv4 } from 'uuid';
export default {
  data(){
    return{
      task:'',
      searchQuery: '',
      color: '',
      todos:[]
    }
  },
  methods:{
    addTask(e){
      e.preventDefault();
      const newTask = {
        id:uuidv4(),
        details:this.task,
        status:false
      }
      this.todos.unshift(newTask);
      this.task = '';
      this.color = '#fff'

    },
    removeTask(id){
      const index = this.todos.findIndex(todo => todo.id === id);
      if (index !== -1) {
        this.todos.splice(index, 1);
      }
    },
    doneTask(id) {
      const task = this.todos.find((todo) => todo.id === id);
      if (task) {
        task.status = !task.status;
      }
    },
    clearAllTasks() {
      this.todos = [];
    },
  },
  computed: {
  filteredTodos() {
    return this.todos.filter(todo => {
      return todo.details.toLowerCase().includes(this.searchQuery.toLowerCase());
    });
  },
  hasTasks() {
    return this.todos.length > 0;
  },
},
}

</script>

<style>
body {
  height: 100vh;
  margin: 0;
  padding: 0;
  background-image: url(./assets/background.jpg);
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  font-family: sans-serif;
}
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
h3 {
  border-radius: 20px;
  font-size: 28px;
  color: white;
  text-shadow: 2px 2px 4px #000000;
  letter-spacing: 1px;
}
.container{
  border:1px solid;
  border-radius: 5px;
  width: 330px;
  padding:20px;
  margin:0 auto;
  text-align: center;
  background-color: #ececec;
  box-shadow: 2px 2px 20px rgba(0, 0, 0, 0.3);
  font-family: sans-serif;
}
.container input[type="text"] {
  height: 30px;
  border: none;
  border-top-left-radius: 20px;
  border-bottom-left-radius: 20px;
  font-size: 20px;
  text-align: center;
  transition: all .3s ease-in-out;
}

.container #input-button {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.container input#search {
  width: 100%;
  margin: 10px auto 10px auto;
  border-radius: 15px;
}
.container input:last-of-type {
  margin-top: 10px;
  margin-bottom: 10px;
}
.container input:first-of-type {
  margin-bottom: 10px;
}
.container input[type="text"]:focus {
  outline: none;
  box-shadow: inset 2px 2px 6px rgba(0, 0, 0, 0.2);
}
.container #add {
  height: 30px;
  width: 100px;
  border: none;
  font-size: 15px;
  background-color: #3498db;
  border-top-right-radius: 20px;
  border-bottom-right-radius: 20px;
  color: #fff;
  cursor: pointer;
  transition: all .3s ease-in-out;
}
.container #add:hover {
  opacity: 0.5;
}
::placeholder {
  color: #bdc3c7;
} 
.task-items{
  display: flex;
  text-align: center;
  color: #fff;
  background-color: #54b4f3;
  padding: 5px 0 5px 0;
  /* width: 270px; */
  width: 100%;
  margin: auto;
  border:1px solid #666;
  margin-bottom: 10px;
  align-items: center;
  text-align: center;
  border-radius: 5px;
  font-size: 15px;
  transition: all .2s linear;
  cursor: pointer;
  height: 30px;
}
.task-items:hover{
  background-color: #a7daee;
  cursor: pointer;
  outline: none;
  box-shadow: inset 2px 2px 6px rgba(0, 0, 0, 0.2);
}
.remove-btn{
  background-color: red;
  color:#fff;
  width: auto;
  height: auto;
  font-size: 14px;
  padding:5px;
  margin-right: 10px;
  cursor: pointer;
  border:1px solid;
  border-radius: 5px;
}
.done-btn{
  background-color: blue;
  color:#fff;
  width: auto;
  height: auto;
  font-size: 14px;
  padding:5px;
  cursor: pointer;
  border:1px solid;
  border-radius: 5px;
  margin-left: auto;
}
.clear-btn{
  background-color: #54b4f3;
  color: #fff;
  width: auto;
  height: 30px;
  font-size: 15px;
  padding:5px;
  cursor: pointer;
  border:1px solid;
  border-radius: 5px;
  margin-left: auto;
  transition: all .3s ease-in-out;
}
.remove-btn:hover, .done-btn:hover, .clear-btn:hover {
  opacity: 0.5;
}
.done{
  text-decoration: line-through;
  color: black;
}
</style>
