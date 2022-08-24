<template>
  <div class="todos">
    <div class="todos-form">
        <h1>Danh sách công việc cá nhân</h1>
        <button type="button" class="btn btn-danger" @click="showModal">Add Todos</button>
        <div class="todos-table">
          <table class="table">
            <thead>
                <th>Stt</th>
                <th>Name</th>
                <th>User Name</th>
                <th>Email</th>
                <th>Sửa/Xoá</th>
            </thead>
            <tbody v-for="(todo,index) in todos" :key="index">
                <th>{{index+1}}</th>
                <th>{{todo.name}}</th>
                <th>{{todo.username}}</th>
                <th>{{todo.email}}</th>
                <th>
                  <span class="edit-btn">
                    <i class="fa-solid fa-pen-to-square" @click="showEditModal(todo,index)"></i>
                  </span>
                  <span class="del-btn">
                    <i class="fa-solid fa-trash-can" @click="delTodos(index)"></i>
                  </span>
                </th>
            </tbody>
          </table>
        </div>
    </div>
  </div>
<!---------add modal  --------->
  <div class="add-modal" v-if="modalAdd">
    <div class="modal-warp">
      <div class="modal-group">
        <p>Name:</p>
        <input type="text" v-model="newTodos.name"/>
      </div>
      <div class="modal-group">
        <p>User Name:</p>
        <input type="text" v-model="newTodos.username"/>
      </div>
      <div class="modal-group">
        <p>Email:</p>
        <input type="text" v-model="newTodos.email"/>
      </div>
      <div class="modal-note">
        <button class="btn note-cancle" @click="showModal">
          Cancle
        </button>
        <button class="btn note-add" @click="addTodos">
          Add
        </button>
      </div>
    </div>
  </div>
  <!-- edit modal -->
   <div class="edit-modal" v-if="modalEdit">
    <div class="modal-warp">
      <div class="modal-group">
        <p>Name:</p>
        <input type="text" v-model="editTodos.name"/>
      </div>
      <div class="modal-group">
        <p>User Name:</p>
        <input type="text" v-model="editTodos.username"/>
      </div>
      <div class="modal-group">
        <p>Email:</p>
        <input type="text" v-model="editTodos.email"/>
      </div>
      <div class="modal-note">
        <button class="btn note-cancle" @click="showEditModal">
          Cancle
        </button>
        <button class="btn note-add" @click="edit">
          Edit
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      todos:[
        {
          name:'Nguyễn Văn Linh',
          username:'Vawn',
          email:'Shanna@melissa.tv',
        }
      ],
      modalAdd:false,
      modalEdit:false,
      newTodos:{
          name:'',
          username:'',
          email:'',
        },
        editTodos:{name:'',username:'',email:'',},
        editIndex:'',
    }
  },
  mounted () {
    axios
      .get('https://jsonplaceholder.typicode.com/users?_limit=5')
      .then(response => {
        this.todos= response.data
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      },
  methods:{
    showModal(){
      this.modalAdd = !this.modalAdd
    },
    addTodos(){
      if(
        this.newTodos.name === '' || this.newTodos.username === '' || this.newTodos.email === ''
      ){
        alert ('Vui lòng nhập đầy đủ dữ liệu!')
        }else {
          axios.post('https://jsonplaceholder.typicode.com/users',this.newTodos)
          .then(response => {
          this.todos.push(response.data)
          console.log(response.data)
          })
          .catch(error => {
          console.log(error)
          this.errored = true
          })
          this.newTodos = {name:'',username:'',email:''}
          this.modalAdd = false
      }
    },
    showEditModal (todo,index) {
      this.modalEdit = !this.modalEdit
      this.editTodos = todo
      this.editIndex = index

    },
    edit (){
      if(confirm('Bạn chắc chắn thay đổi?')){
        this.todos[this.editTodos] = this.editTodos
        this.modalEdit = false
      }
    },
    delTodos (index) {
      if(confirm('Bạn chắc chắn xoá không?')) {
      axios
      .delete(`https://jsonplaceholder.typicode.com/users/${index}`)
      .then(response => {
        this.todos = response.data
        this.response.data.splice(index,1)
      })
      .catch(error => {
      console.log(error)
      this.errored = true
      })
      }
    }
  }
}
</script>

<style lang="scss">
    .todos{
      width: 100%;
      position: relative;
      padding-bottom:30px;
        &-form{
          h1 {
            text-align: center;
            padding: 20px;
            font-weight: 500;
            letter-spacing: 4px;
            color: rgb(7, 45, 83);
          }
          .btn-danger{
            display:block;
            margin: 12px auto;
          }
          .todos-table{
            margin:0 auto 0 auto;
            width: 85%;
            margin-bottom:100px;
            background-color: #fff;
            text-align:center;
            padding: 8px;
            box-shadow: rgba(3, 102, 214, 0.3) 0px 0px 0px 3px;
            .table {
              padding: 50px;
              thead{
                th{
                  border:2px solid #ccc;
                }
              }
              tbody{
                th{
                  border:2px solid #ccc;
                  
                  .edit-btn {
                    padding: 8px 12px;
                    background-color:burlywood;
                    cursor: pointer;
                  }
                  .edit-btn:hover {
                    opacity: .7;
                  }
                  .del-btn {
                    padding: 8px 12px;
                    background-color: cadetblue;
                    cursor: pointer;
                  }
                  .del-btn:hover {
                    opacity: .7;
                  }
                }
              } 
            }
          } 
        }
    }
    .add-modal, .edit-modal {
      position: fixed;
      z-index: 2;
      top: 0;
      right: 0;
      left: 0;
      width: 100%;
      height: 100vh;
      display:flex;
      align-items: center;
      justify-content: center;
      background-color: rgba(0, 0, 0, .4);
      .modal-warp {
        display:flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        width: 50%;
        height: 300px;
        background-color:aqua;
        padding: 200px 30px;
        box-shadow: rgb(85, 91, 255) 0px 0px 0px 3px, rgb(31, 193, 27) 0px 0px 0px 6px, rgb(255, 217, 19) 0px 0px 0px 9px, rgb(255, 156, 85) 0px 0px 0px 12px, rgb(255, 85, 85) 0px 0px 0px 15px;
        .modal-group {
          display:flex;
          margin: 8px;
          p {
            min-width: 100px;
            color:rgb(74, 1, 1);
          }
          input {
            outline: none;
            border:1px solid #ccc;
            border-radius: 8px;
            width:400px;
            padding: 8px;
          } 
        }
        .modal-note {
          align-self: flex-end;
          margin-right: 58px;
          .note-cancle {
            margin-right: 8px;
            background-color: darkblue;
            color:#fff;
          }
          .note-add {
            width: 77px;
            background-color: mediumslateblue;
            color:#fff;
          }
        }
      }
    }
</style>