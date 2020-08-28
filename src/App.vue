<template>
  <div id="app">
    <h1>todos</h1>

    <div class="container">
      <div class="row">
        <div class="col-12">
          <div class="card">
            <div class="card-header">
              <div class="input-group mb-3">
                <div class="input-group-prepend">
                  <button class="btn btn-warning" v-if="tasks.length > 0 && allCompleted" @click="unCheckAll">uncheck all</button>
                  <button class="btn btn-success" v-else @click="checkAll">check all</button>
                </div>
                <input type="text" class="form-control" v-model="input" @keypress.enter="addTask">
              </div>
            </div>
            <div class="card-body">

              <span v-if="filteredTasks.length === 0">no tasks</span>

              <div class="input-group mb-3" v-for="task in filteredTasks" v-bind:key="task.id">
                <div class="input-group-prepend">
                  <div class="input-group-text">
                    <input type="checkbox" aria-label="Checkbox for following text input" v-model="task.completed">
                  </div>
                </div>
                <span class="form-control text-left">
                  <span v-if="!task.completed">{{ task.task }}</span>
                  <del v-else>{{ task.task }}</del>
                </span>
                <div class="input-group-append">
                  <button class="btn btn-danger" @click="deleteTask(task.id)">remove</button>
                </div>
              </div>

            </div>
            <div class="card-footer">
              {{ itemsLeft }} item(s) left
              <div class="mx-4 d-inline-block">
                <button class="btn mr-2" :class="{'btn-primary': filter === 'all'}" @click="filter = 'all'">all</button>
                <button class="btn mr-2" :class="{'btn-primary': filter === 'active'}" @click="filter = 'active'">
                  active
                </button>
                <button class="btn" :class="{'btn-primary': filter === 'completed'}" @click="filter = 'completed'">
                  completed
                </button>
              </div>
              <button class="btn btn-danger" @click="clearCompleted">clear completed</button>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>

export default {
  name: 'App',
  components: {},
  data() {
    return {
      id: 0,
      tasks: [],
      filter: 'all',
      input: '',
    }
  },
  methods: {
    addTask() {
      this.tasks.push({
        id: this.id,
        task: this.input,
        completed: false,
      })

      this.id++
      this.input = ''
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter(el => el.id !== id)
    },
    clearCompleted() {
      this.tasks = this.tasks.filter(el => !el.completed)
    },
    checkAll() {
      this.tasks = this.tasks.map(el => {
        el.completed = true
        return el
      })
    },
    unCheckAll() {
      this.tasks = this.tasks.map(el => {
        el.completed = false
        return el
      })
    },
  },
  computed: {
    itemsLeft() {
      return this.tasks.filter(el => !el.completed).length
    },
    filteredTasks() {
      if (this.filter === 'completed') {
        return this.tasks.filter(el => el.completed);
      } else if (this.filter === 'active') {
        return this.tasks.filter(el => !el.completed);
      }

      return this.tasks;
    },
    allCompleted() {
      return this.tasks.filter(el => !el.completed).length === 0
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
