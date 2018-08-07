<template>
  <section class="todoapp">
    <todo-header></todo-header>
    <section class="main" v-if="todos.length">
      <input type="checkbox" class="toggle-all" @click="allDone">
      <label for="toggle-all">Mark all as complete</label>
      <ul class="todo-list">
        <li v-for="todo in todos" v-bind:key="todo" :class="{'completed': todo.completed, 'editing': todo === editedTodo}">
          <div class="view">
            <input type="checkbox" class="toggle" v-model="todo.completed">
            <label @dblclidk="editTodo(todo)">{{ todo.title }}</label>
            <button class="destroy" @click="removeTodo(todo)"></button>
          </div>
          <input type="text" class="edit" v-model="todo.title" v-todo-focus="todo == editedTodo" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)">
        </li>
      </ul>
    </section>
    <todo-footer></todo-footer>
  </section>
</template>

<script>
import TodoHeader from '~/components/header'
import TodoFooter from '~/components/footer'

export default {
  validate ({ params }) {
    return !params.slug || params.slug === 'active' || params.slug === 'completed'
  },
  head () {
    return {
      title: this.$route.params.slug || 'all',
      titleTemplate: 'Nuxt TodoMVC : %s todos'
    }
  },
  data () {
    return {
      editedTodo: null
    }
  },
  watch: {
    todos: {
      deep: true,
      handler: 'save',
    }
  },
  computed: {
    todos () {
      if (this.$route.params.slug === 'active') {
        return this.$store.getters.activeTodos
      }
      if (this.$route.params.slug === 'completed') {
        return this.$store.getters.completedTodos
      }
      return this.$store.getters.allTodos
    }
  },
  methods: {
    allDone () {
      this.$store.dispatch('allDone')
    },
    editTodo (todo) {
      this.beforeEditCache = todo.title
      this.editedTodo = todo
    },
    doneEdit (todo) {
      this.editedTodo = null
      todo.title = todo.title.trim()
      if (!todo.title) {
        this.$store.dispatch('removeTodo', todo)
      }
    },
    cancelEdit (todo) {
      this.editedTodo = null
      todo.title = this.beforeEditCache
    },
    removeTodo (todo) {
      this.$store.dispatch('removeTodo', todo)
    },
    save () {
      this.$store.dispatch('saveTodos')
    }
  },
  directives: {
    'todo-focus' (el, binding) {
      if (binding.value) {
        el.focus()
      }
    }
  },
  components: {
    TodoHeader, TodoFooter
  }
}
</script>

