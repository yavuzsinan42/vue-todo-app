<template>
  <div class="container">
    <div class="row">
      <div class="col-md-8 offset-md-2 text-center">
        <h3 class="mt-5">TODO List / Vue.JS</h3>
        <hr>
        <div class="row">
          <div class="col-md-6 offset-md-3 d-flex justify-content-between p-3 ">
            <input type="text" class="form-control mx-5" v-model="todoText">
            <button class="btn btn-success" @click="addTodo()">Ekle</button>
          </div>
        </div>
        <div class="todo-container">
          <!-- eslint-disable-next-line -->
          <TodoComp  @deleteTodo="deleteTodo($event)" v-for="todo in todoList" :todo="todo" class="my-2"/>
        </div>
      </div>
    </div>
    
  </div>
</template>

<script>
 /* eslint-disable */
import TodoComp from './components/Todo-comp.vue'
import axios from "axios"
export default {
  components: {
    TodoComp
  },
  data() {
    return {
      todoText: "",
      todoList: []
    }
  },
  methods: {
    addTodo() {
      axios.post("https://httprequest-d3cdd-default-rtdb.firebaseio.com/todoList.json", { text: this.todoText }).
        then(response => {
          console.log(response.data.name);
          this.todoList.push({id: response.data.name, text: this.todoText})

        }).
        catch(e => console.log(e))
    },
    deleteTodo(todoId){
      axios.delete("https://httprequest-d3cdd-default-rtdb.firebaseio.com/todoList/"+todoId+".json")
      .then(response => {
        let index = this.todoList.findIndex(i => {
          return i.id == todoId
        })
        this.todoList.splice(index, 1)
        console.log(response);
      })
    }
    
  },
  created(){
      axios.get("https://httprequest-d3cdd-default-rtdb.firebaseio.com/todoList.json").then(response => {
          console.log(response.data);
          for(let key in response.data){
            console.log(response.data[key]);
            let todo = {
              text: response.data[key].text,
              id: key

            }
            this.todoList.push(todo)
          }
        }).
        catch(e => console.log(e))
    }
}
</script>

<style>

</style>
