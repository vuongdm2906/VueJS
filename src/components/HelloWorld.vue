<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
  <div>
    <input type="text" v-model="value">
    <button @click="handleTodoList">ADD</button>
  </div>
  <div>
    <ul v-for="(value, index) in todoList" :key="index">
      <li><span :class="{ done: value.status }" @click="handleStatus(value)">{{ value.content }}</span><button
          @click="remove(index)">Xoa</button></li>
    </ul>
  </div>
  <div>
    <h5 v-show="completedTask.length >= 1">Danh sach cong viec hoan thanh</h5>
    <ul v-for="(value, index) in completedTask" :key="index">
      <li><span :class="{ done: value.status }" @click="handleStatus(value)">{{ value.content }}</span></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      value: '',
      todoList: [],
    }
  },
  computed: {
    completedTask() {
      return this.todoList.filter(value => value.status)
    }
  },
  watch: {
    todoList: {
      handler(value) {
        localStorage.setItem('todoListStorage', JSON.stringify(value))
      }
    }
  },
  mounted() {
    const todoList = localStorage.getItem('todoList')
    if(todoList)
    {
      this.todoList = JSON.parse(todoList)
    }
  },
  methods: {
    handleTodoList() {
      if (this.value.trim() !== '') {
        this.todoList.push({ content: this.value, status: false })
        this.value = ''
      }
    },
    handleStatus(value) {
      value.status = !value.status
    },
    remove(index) {
      this.todoList.splice(index, 1)
    }
  }
}
</script>


<style scoped>
h3 {
  margin : 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.done {
  text-decoration: line-through;
}
</style>