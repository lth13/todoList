<template>
    <div class="container">
      <div>
        <div class="input-todos">
          <input class="input-todo" v-model="newTodo" @keyup.enter="addTodo()" placeholder="What needs to be done?">
        </div>
        <div class="list-todos-container">
          <div class="list-todos" v-for="(item, index) in listTodoss" :key="item.id">
            <input class="item-checkbox" type="checkbox" v-model="item.complete" @change="checkedItem(item)">
            <div class="item-title">{{item.title}}</div>
            <div class="delete-item" @click= "remove(index)">-</div>
          </div>
        </div>
      </div>
      <div class="footer">
        <input type="checkbox" class="check-all-items" @change="checkAll()">
        <div class="all-items-title">All items</div>
        <button class="all-items" @click="showAll()">All</button>
        <button class="complete-items" @click="showCompleted()">Completed</button>
        <button class="incomplete-items" @click="showIncomplete()">Incomplete</button>
        <button class="clear-item" @click="clearItem()">Clear</button>
      </div>
    </div>
</template>

<script>
export default {
  name: 'todo',
  data () {
    return {
      newTodo: '',
      id: 1,
      listTodos: [],
      listTodoss: JSON.parse(localStorage.getItem('todo'))
    }
  },
  created () {
  },
  computed: {},
  methods: {
    addTodo () {
      if (this.newTodo.trim() !== '') {
        this.listTodos.push({
          'id': this.id,
          'title': this.newTodo,
          'complete': false
        })
        this.id++
        this.newTodo = ''
        localStorage.setItem('todo', JSON.stringify(this.listTodos))
        this.listTodoss = JSON.parse(localStorage.getItem('todo'))
      }
    },
    checkedItem (item) {
      this.listTodos = JSON.parse(localStorage.getItem('todo'))
      //
      this.listTodos.forEach(function (x) {
        if (x.id == item.id) {
          x.complete = !x.complete
        }
      })
      localStorage.setItem('todo', JSON.stringify(this.listTodos))
      this.listTodoss = JSON.parse(localStorage.getItem('todo'))
    },
    remove  (index) {
      this.listTodos = JSON.parse(localStorage.getItem('todo'))
      this.listTodos.splice(index, 1)
      localStorage.setItem('todo', JSON.stringify(this.listTodos))
      this.listTodoss = JSON.parse(localStorage.getItem('todo'))
      this.listTodoss = this.listTodos
    },
    checkAll () {
      this.listTodos = JSON.parse(localStorage.getItem('todo'))
      this.listTodos.forEach(item => item.complete = event.target.checked)
      localStorage.setItem('todo', JSON.stringify(this.listTodos))
      this.listTodoss = JSON.parse(localStorage.getItem('todo'))
    },
    showAll () {
      this.listTodoss = this.listTodos
    },
    showCompleted () {
      this.listTodoss = this.listTodos.filter(x => x.complete === true)

    },
    showIncomplete () {
      this.listTodoss = JSON.parse(localStorage.getItem('todo'))
      this.listTodoss = this.listTodos.filter(x => x.complete !== true)
    },
    clearItem: function () {
      this.listTodos = JSON.parse(localStorage.getItem('todo'))
      this.listTodos = this.listTodoss.filter(x => x.complete === false)
      localStorage.setItem('todo', JSON.stringify(this.listTodos))
      this.listTodoss = JSON.parse(localStorage.getItem('todo'))
    }
  }
}
</script>

<style scoped>
  .container{
    width: 400px;
    min-height: 80px;
    margin: 0px auto;
    border: 0.5px solid black;
    border-radius: 10px;
  }
  .input-todos{
    display: flex;
    justify-content: space-around;
    margin-top: 10px;
  }
  .input-todo{
    width: 80%;
    padding-left: 15px;
  }
  .list-todos-container{
  }
  .list-todos{
    width: 85%;
    margin: 0px auto;
    display: flex;
    font-family: sans-serif;
    margin-top: 10px;
    margin-bottom: 10px;
  }
  .item-checkbox{
    width: 5%;
  }
  .item-title{
    width: 90%;
  }
  .delete-item{
    width: 5%;
  }
  .delete-item:hover {
    cursor: pointer;
  }
  .footer{
    width: 85%;
    margin: 0px auto;
    display: flex;
    font-family: sans-serif;
    font-size: 10px;
    justify-content: space-between;
    border-top: 1px solid black;
    padding-top: 10px;
    padding-bottom: 10px ;
  }
  .all-items-title{
    font-size: 15px;
    padding-top: 2px;
  }
  .footer button{
  }

</style>
