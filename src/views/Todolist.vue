<template>
   <div>
     <br>
         <b-row class="mt-5 d-flex justify-content-center">
           <b-col cols="6">
             <b-card bg-variant="grey" text-variant="black">
                <h2>{{message}}</h2>
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
                      placeholder="Description.."
                      rows="3" max-rows="3">
                      </b-form-textarea>
                      <b-button block variant="info" type="submit" v-if="form.id" @click="onHandLeUpdate()">Update</b-button>
                      <b-button block variant="outline-secondary" type="submit" v-if="form.id" @click="onHandLeCancel()" >cancel</b-button>
                      <b-button block variant="primary" @click="onHandLeAdd()" type="submit"  v-else>Add Task</b-button>
                  <br>
                
              </form>
             </b-card>
           </b-col>
         </b-row>
     
          <b-row class="mt-5 d-flex justify-content-center">
           <b-col cols="6">
               <form>
                
                <b-card 
                      v-for="(todo) in todos"
                      :key = "todo.id" header-tag="header">
                         <b-row>
                        <b-col align="left"> {{ todo.title }}</b-col>
                        <b-col align="left">  {{ todo.description }} </b-col>
                        <b-col align="right">  
                                
                                <b-btn variant="info" @click="onHandLeClickUpdate(todo)" > 
                                  <span class="fa fa-edit"></span>
                                </b-btn>
                                <b-btn variant="danger" @click="onHandLeClickDelete(todo.id)" >
                                  <span class="fa fa-trash"></span>
                                </b-btn>
                        </b-col>
                        </b-row>
                  </b-card>
                  
            </form>
          </b-col>
          </b-row>
    </div>
</template>

<script>
// import { title } from 'process'

export default {
      name: "Home",
      data(){
        return{
              message: "To-Do List",
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
            },

            onHandLeCancel(){
                  
                                this.form ={
                                  title:'',
                                  description:''
                                }
                
          }
      }
  }
</script>

<style scoped>

</style>
