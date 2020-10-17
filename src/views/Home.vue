<template>
   <div  class="row">
 
    <div class="col-12 py-5">
        <!-- <h2>{{message}}</h2> -->

         <form>
            <b-input-group>
                    <input
                        v-model="form.title"
                        type="text"
                        class="form-control"
                        placeholder="Add new task..."
            />
      
              </b-input-group>
                <b-form-textarea id="textarea"
                  v-model="form.description"
                  placeholder="Enter todo description.."
                  rows="3" max-rows="6">
                  </b-form-textarea>
                  <b-button variant="outline-secondary" type="submit" v-if="form.id" @click="onHandLeUpdate()">Update</b-button>
                  <b-button variant="outline-secondary" @click="onHandLeAdd()" type="submit"  v-else>Add Task</b-button>
              <br>
             
          </form>
          <form>
             <b-card 
                  v-for="(todo) in todos"
                  :key = "todo.id" header-tag="header">
                  <template v-slot:header>
                    <h6 class="mb-0">{{ todo.title }}</h6>
                  </template>
                  <b-card-text>
                    {{ todo.description }}
                    <b-btn variant="danger" @click="onHandLeClickDelete(todo.id)" >Delete</b-btn>
                    <b-btn variant="info" @click="onHandLeClickUpdate(todo)" >Update</b-btn>
                  </b-card-text>
              </b-card>
          </form>
      </div>
  
    </div>
</template>

<script>
// import { title } from 'process'

export default {
      name: "Home",
      data(){
        return{
              todos:[],
              form: {
                title:'',
                description: ''
              }
        }
      },

      mounted(){
          this.getTodos() //call back method todos
      },

      methods: {
            getTodos(){
                  this.$http.get('http://localhost:3000/todos')
                      .then(res => {
                          this.todos = res.data
                      })
            },
            onHandLeAdd(){
              this.$http.post('http://localhost:3000/todos', this.form)
                                  .then(() => {
                                      this.form ={
                                  title:'',
                                  description:''
                                      }
                                    this.getTodos()
                                  })  
            },
            onHandLeUpdate(){
              this.$http.patch(`http://localhost:3000/todos/${this.form.id}`, this.form)
                                  .then(() => {
                                      this.form ={
                                  title:'',
                                  description:''
                                      }
                                    this.getTodos()
                                  })  
            },
            onHandLeClickDelete(id){
              this.$http.delete(`http://localhost:3000/todos/${id}`) 
                              .then(() => {
                                this.form ={
                                  title:'',
                                  description:''
                                }
                                    this.getTodos()
                                  })               
            },
            onHandLeClickUpdate(todo){
            this.form = todo
            this.getTodos()         
            }
          
                
          }
  }
</script>

<style scoped>

</style>
