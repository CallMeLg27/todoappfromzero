<template>
  <b-modal
    id="modal-1"
    title="Modo edición"
    @ok="handleOk()"
  >
    <b-form @submit="handleSubmit">
      <b-form-group
        id="input-group-2"
        label="TODO:"
        label-for="input-2"
      >
        <b-form-input
          id="todo-text"
          required
          :placeholder="this.todo.text"
          v-model="todoText"
        ></b-form-input>
      </b-form-group>
    </b-form>
    <button @click="debugTodoText()"></button>
  </b-modal>
</template>

<script>
export default {
  props: ['todo'],
  data () {
    return {
      todoText: ""
    }
  },
  methods: {
    debugTodoText () {
      //   alert(this.todo.text)
      alert(this.todoText)
    },
    handleSubmit (submitevt) {
      submitevt.preventDefault()
      this.$nextTick(() => {
        this.$bvModal.hide('modal-1')
      })
      this.handleOk()
    },
    handleOk () {
      this.$emit('update-todo', this.todo.id, this.todoText)
      this.todoText = ""
    }
  },
}
</script>

<style>
</style>