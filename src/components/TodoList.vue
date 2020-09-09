<template>
  <div>
    <AddTodo 
      @add-todo="addTodo"
      @title-changed="titleChanged"
    />
    
    <ul v-if="todos.length>0">
      <TodoItem v-for="(todo, index) in todos" :key="index"
        :todo = todo
        :index = index
        @remove-todo = "removeTodo"
        @selection-toggled = "updateSelected"
      />
    </ul>
    <h2 v-else align="center">No todos</h2>

    <div class="todo-footer">
      <div class="items_left">{{todos.filter(t => !t.completed).length}} items left</div>
      <div class="control-buttons">
        <div class="btn btn-secondary" @click="$emit('set-filter', 'all')">All</div>
        <div class="btn btn-secondary" @click="$emit('set-filter', 'active')">Active</div>
        <div class="btn btn-secondary" @click="$emit('set-filter', 'completed')">Completed</div>
        <div class="btn btn-secondary clear" @click="$emit('remove-completed')">Clear All Completed</div>
      </div>
      
    </div>
  </div>
</template>

<script>
import TodoItem from '@/components/TodoItem.vue'
import AddTodo from '@/components/AddTodo.vue'

export default {
  name: 'TodoList',
  components: {
    TodoItem, AddTodo
  },
  data() {
    return {
      arrSelected: []
    }
  },
  props: ['todos'],
  methods: {
    removeTodo(id) {
      this.$emit('remove-todo', id)
    },
    addTodo(todo) {
      this.$emit('add-todo', todo)
    },
    titleChanged(title) {
      this.$emit('title-changed', title)
    },
    updateSelected(id, isSelected) {
      if (isSelected) {
        if (this.arrSelected.indexOf(id) === -1) {
          this.arrSelected.push(id)
        }
      } else {
        let indexOfId = this.arrSelected.indexOf(id)
        if (indexOfId !== -1) {
          this.arrSelected.splice(indexOfId, 1)
        }
      }
      // передаем его еще выше to Parent
      this.$emit('array-of-selected', this.arrSelected)
    },
    clearSelected() {
      this.arrSelected.length = 0
      // to Parent
      this.$emit('array-of-selected', [])
      // to Children
      this.$emit('clear-selected', true)
    }
  },
  created() {
    this.$parent.$on('clear-selected', this.clearSelected)
  }
}
</script>

<style scoped>
ul {
  list-style: none;
  padding: 0;
}

.todo-footer {
  display: flex;
  font-size: 0.8em;
  align-items: center;
  justify-content: space-evenly;
  padding-top: 5px;
  border-top: 1px solid #ccc;
}

.items_left {
  font-weight: bold;
  white-space: nowrap;
}

.control-buttons {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-end;
  margin-left: 10px;
}

.control-buttons.btn-secondary {
  margin: 0 5px;
}

.btn-secondary {
  color: #555;
  border-radius: 3px;
  border: 0;
  padding: 10px;
}

.btn-secondary:hover {
  color: #4442f6;
  background: #4442f610;
}

.btn-secondary:active {
  color: #4442f6;
  background: #4442f620;
}

.btn-secondary.clear:hover {
  color: #f56468;
  background: #f5646810;
}

.btn-secondary.clear:active {
  color: #f56468;
  background: #f5646820;
}
</style>