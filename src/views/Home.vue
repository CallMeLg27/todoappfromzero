<template>
  <div class="home">
    <transition name="fade">
      <EditTodoModal
        :todo="selectedTodo"
        @update-todo="updateTodoText"
      ></EditTodoModal>
    </transition>
    <div class="mx-auto w-75">
      <h1>Lista de tareas</h1>
      <div>
        <b-form @submit="createTodo">
          <b-form-input
            v-model="todoText"
            placeholder="¿Que haremos hoy?"
          ></b-form-input>
        </b-form>
      </div>
      <b-list-group>
        <b-list-group-item
          class="big"
          v-if="todos.length===0"
        >
          Parece que no hay ninguna tarea por aquí <b-icon-emoji-frown></b-icon-emoji-frown>
        </b-list-group-item>
        <b-list-group-item
          class="big"
          v-if="applyFilter(todos,currentFilter).length===0 && currentFilter=='done' && todos.length>0"
        >
          Aún no has terminado ninguna tarea <b-icon-emoji-frown></b-icon-emoji-frown>
        </b-list-group-item>
        <b-list-group-item
          class="big"
          v-if="applyFilter(todos,currentFilter).length===0 && currentFilter=='not done' && todos.length>0"
        >
          Has terminado todas tus tareas. ¡Felicidades! <b-icon-emoji-sunglasses></b-icon-emoji-sunglasses>
        </b-list-group-item>
        <b-list-group-item
          v-for="todo in applyFilter(todos,currentFilter)"
          :key="todo.id"
        >
          <div class="d-flex">
            <div class="p-2">
              <b-form-checkbox
                switch
                size="lg"
                v-model="todo.done"
              ></b-form-checkbox>
            </div>
            <div
              class="p-2"
              :class="{'done':todo.done}"
            >{{todo.text | capitalize}}</div>
            <div
              class="p-2 small"
              :class="{'done':todo.done}"
            >{{todo.createdOn | renderDate}}</div>
            <div
              class="p-2 small"
              :class="{'done':todo.done}"
            >{{todo.deadline | renderDate}}</div>
            <div class="ml-auto p-2">
              <b-button
                size="sm"
                @click="setCurrentTodo(todo)"
                v-b-modal.modal-1
              >
                <b-icon-pencil-square></b-icon-pencil-square>
              </b-button>
            </div>
          </div>
        </b-list-group-item>
      </b-list-group>
      <div class="d-flex">
        <b-button
          pill
          variant="primary"
          class="m-2"
          @click="currentFilter='all'"
        >Todas</b-button>
        <b-button
          pill
          variant="danger"
          class="m-2"
          @click="currentFilter='not done'"
        >Por hacer</b-button>
        <b-button
          pill
          variant="success"
          class="m-2"
          @click="currentFilter='done'"
        >Terminadas</b-button>
        <b-button
          pill
          variant="light"
          class="ml-auto m-2"
          @click="removeDone()"
        >Borrar terminadas</b-button>
      </div>
    </div>
  </div>
</template>

<script>

// @ is an alias to /src
import EditTodoModal from '@/components/EditTodoModal.vue'
import moment from 'moment'

export default {
  name: 'Home',
  components: {
    EditTodoModal
  },
  data () {
    return {
      todos: [
        { "id": 1, "text": "todo 1", "createdOn": "2014-02-01T09:28:56.321-10:00", "deadline": "2014-02-01T09:28:56.321-10:00", "done": false },
        { "id": 2, "text": "todo 2", "createdOn": "2014-02-01T09:28:56.321-10:00", "deadline": "2014-02-01T09:28:56.321-10:00", "done": true },
        { "id": 3, "text": "todo 3", "createdOn": "2014-02-01T09:28:56.321-10:00", "deadline": "2014-02-01T09:28:56.321-10:00", "done": false },
        { "id": 4, "text": "todo 4", "createdOn": "2014-02-01T09:28:56.321-10:00", "deadline": "2014-02-01T09:28:56.321-10:00", "done": false }
      ],
      todoText: "",
      selectedTodo: {},
      currentFilter: "all"
    }
  },
  methods: {
    createTodo (evt) {
      evt.preventDefault()
      var newTodo = { "id": this.todos.length + 1, "text": this.todoText, "done": false }
      this.todos = [...this.todos, newTodo]
      this.todoText = ""
    },
    updateTodoText (todoId, newText) {
      if (newText == "") {
        return
      }
      this.todos[todoId - 1].text = newText
    },
    setCurrentTodo (todo) {
      this.selectedTodo = todo
      // alert(this.selectedTodo.text)
    },
    applyFilter (todos, currfilter) {
      if (currfilter == "all") {
        return todos
      }
      else if (currfilter == "not done") {
        return this.onlyShowNotDone(todos)
      }
      else if (currfilter == "done") {
        return this.onlyShowDone(todos)
      } else {
        return [{ "error": "el filtro no es valido" }]
      }
    },
    onlyShowNotDone (todos) {
      return todos.filter(todo => !todo.done)
    },
    onlyShowDone (todos) {
      return todos.filter(todo => todo.done)
    },
    removeDone () {
      this.todos = [...this.onlyShowNotDone(this.todos)]
    },
    foo () {
      alert('foo')
    }
  },
  filters: {
    capitalize (value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    },
    renderDate (value) {
      return moment(String(value)).format('MM / DD / YYYY hh: mm')
    }
  }
}

</script>

<style scoped>
.done {
  text-decoration: line-through;
  font-weight: 100;
}
</style>
