<template>
  <header class="header">
    <h1>todos</h1>
    <input type="text" class="new-todo"
           placeholder="What needs to be done?"
           autofocus v-model="todo" @keyup.enter="addTodo">
  </header>
</template>

<script>
export default {
  data () {
    return { todo: '' }
  },
  methods: {
    addTodo () {
      const value = this.todo && this.todo.trim()
      if (value) {
        this.$store.dispatch('addTodo', {
          title: value,
          completed: this.$route.params.slug === 'completed'
        })
        this.todo = ''
      }
    }
  }
}
</script>
