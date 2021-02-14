<template>
    <div id="tasks-comp">
        <div class="row d-flex">
            <div class="form-div mb-5 col-md-6">
                <h1>Add You Todo</h1>
                <form @submit.prevent="addTask">
                    <div class="form-group">
                        <label for="description"><i class="far fa-sticky-note mr-3"></i>DESCRIPTION</label>
                        <textarea class="form-control" v-model="description" id="description" rows="4" name="description"></textarea>
                    </div>
                     <div class="form-group">
                        <label for="status">Example select</label>
                        <select v-model="status" class="form-control" id="status">
                        <option value="todo">Todo</option>
                        <option value="done">Done</option>
                        </select>
                    </div>
                    <button type="submit" class="submit shadow-lg mt-3"><i class="far fa-paper-plane mr-2"></i>ADD TODO</button>
                </form>
            </div>
            <div class="col-md-6 svg-div mb-5">
                <img class="svg-task" src="../assets/task.svg" alt="">
            </div>
        </div>
        <hr class="bg-warning text-bold py-1 rounded mt-3">
        <div class="row todo d-flex justify-content-center">
            <div class="col-md-5 m-3 todo-list">
                <h2>You have {{ActiveTodoTaskLength}} {{ ActiveTodoTaskLength === 1 ? 'Todo': "Todo's"}}</h2>
                <form @submit.prevent="DoneTask(task.id, task.description)" v-for="task in tasks" v-show="task.status === 'todo'" :key="task.id" method="post" class="todo-list-form">
                    <label class="rounded-top" for=""><div>{{task.description}}</div>  <a class="mr-3" @click="DeleteTodo(task.id)">
                            Delete
                        </a></label>
                    <button type="submit"><i class="fas fa-check-double"></i> DONE</button>
                </form>
            </div>
            
            <div class="col-md-5 m-3 todo-done">
                <h2>You have {{DoneTodoTaskLength}} {{ DoneTodoTaskLength === 1 ? 'Todo': "Todo's"}}</h2>
                <ul class="done-list">
                    <li v-for="task in tasks" :key="task.id" v-show="task.status === 'done'" class="rounded-top">
                        <div>
                            {{task.description}}
                        </div>
                        <a class="mr-3" @click="DeleteTodo(task.id)">
                            Delete
                        </a>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
    return {
      tasks: [],
      description: '',
      status: 'todo',
    }
  },
  computed: {
      ActiveTodoTaskLength() {
          return this.tasks.filter(task => task.status === 'todo').length
      },
      DoneTodoTaskLength() {
          return this.tasks.filter(task => task.status === 'done').length
      }
  },
  methods: {
    getTasks() {
      axios ({
        method: 'get',
        url: 'http://127.0.0.1:8000/api/tasks/',
        auth: {
          username: 'pkkarn',
          password: 'pkkarn'
        }
      }).then(response => this.tasks = response.data)
    },
    DeleteTodo(id) {
    
        const task = this.tasks.filter(task => task.id !== id)
        this.tasks = task 
       
        axios({
            method: 'delete',
            url: `http://127.0.0.1:8000/api/delete/${id}/`,
            auth: {
                username: 'pkkarn',
                password: 'pkkarn'
            }
        }).then(() => {
            console.log('deleted')
        })
    },
    DoneTask(id, desc) {
        // axios.put(`http://127.0.0.1:8000/api/delete/${id}/`, {
        //     status: 'done'
        // }).then(res => {
        //     console.log(res)
        //     this.getTasks()
        // })
        axios({
            method: 'put',
            url: `http://127.0.0.1:8000/api/delete/${id}/`,
            headers: {
                'Content-Type': 'application/json',
            },
            data: {
                "description": desc,
                "status": "done"
            },
            auth: {
                username: 'pkkarn',
                password: 'pkkarn'
            },
        }).then((response) => {
            this.getTasks()
            console.log(response)
        })
    },
    addTask() {
        if(this.description) {
            axios({
                method: 'post',
                url: 'http://127.0.0.1:8000/api/tasks/',
                data: {
                    description: this.description,
                    status: this.status
                },
                auth: {
                    username: 'pkkarn',
                    password: 'pkkarn'
                }
            }).then((response) => {
                let newTask = {
                    'id': response.data.id,
                    'description': this.description,
                    'status': this.status
                }

                this.tasks.push(newTask)
                this.tasks = this.tasks.reverse()
                this.description = ''
                this.status = 'todo'
            }).catch(error => {
                console.log(error)
            })
        }
    }
  },
  mounted() {
    this.getTasks()
    console.log(this.tasks)
  },
   
    
}
</script>

<style lang="scss">
$primary-blue: #010148;
$primary-yellow: rgb(255, 207, 87);
    #tasks-comp {
        .form-div {
            display: flex;
            align-items: center;
            justify-content: center;
            flex-direction: column;
            h1 {
                font-size:30px;
                font-weight: bold;
                text-transform: uppercase;
                border-bottom: 1px solid #777;
                width: 90%;
                margin-bottom: 30px;
                color: $primary-blue;
            }
            
        form {
            width: 90%;
            label {
                font-size: 18px;
                font-weight: bold;
                color: $primary-blue;
            }
            select {
                cursor: pointer;
            }
            textarea, select{
                border: 1px solid $primary-blue;
                &:focus {
                    border: 2px solid $primary-blue!important;
                }
            }
            button {
                width: 100%;
                padding: 17px;
                background: $primary-yellow;
                border: 2px solid lighten($primary-blue,5%);
                border-radius: 5px;
                color: #333;
                font-weight: bold;
                box-shadow: 2px 2px solid black;
                &:focus {
                    outline: none;
                }
            }
        }
        }
        .svg-div{
            display: flex;
            align-items: center;
            justify-content: center;
                box-shadow: 2px 2px 15px solid black;

        }
        .svg-task {
            width: 80%;
        }

        .todo {
            &-list {
                padding: 0;
                &-form {
                    // border: 1px solid #333;
                    display: flex;
                    flex-direction: column;

                    label {
                        display: flex;
                        justify-content: space-between;
                        a {
                            color: red;
                            font-weight: bold;
                            background: #fff;
                            padding: 2px 10px;
                            border-radius: 8px;
                            cursor: pointer;
                            text-decoration: none;
                        }
                        width: 100%;
                        // height: 10px;
                        padding: 10px;
                        background-color: lighten($primary-yellow,20%);
                        margin-bottom: 0;
                    }
                    button {
                        width: 100%;
                        padding: 10px;
                        background-color: $primary-blue;
                        color: #fff;
                        font-weight: bold;
                        font-family: 'Poppins',sans-serif;
                        letter-spacing: 2px;
                        border: none;
                        margin-bottom: 30px;
                    }
                }
            }
            &-done {
                padding: 0;
                .done-list {
                    list-style-type: none;
                    li {
                        display: flex;
                        align-items: center;
                        padding-left: 10px;
                        justify-content: space-between;
                        height: 40px;
                        border-bottom: 2px solid lighten($primary-yellow,10%);
                        margin-bottom: 12px;
                        background-color: rgb(250, 104, 104);
                        color: #fff;
                        font-weight: 400;
                        cursor: pointer;
                        a {
                            color: red;
                            font-weight: bold;
                            background: #fff;
                            padding: 1px 10px;
                            border-radius: 8px;
                            cursor: pointer;
                            text-decoration: none;
                        }
                    }
                }
            }
            
        }
    }

</style>