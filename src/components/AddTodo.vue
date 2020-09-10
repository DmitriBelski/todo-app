<template>
  <div>
    <form @keydown.enter.prevent="">
      <input type="text" class="input-todo" :class="{ active: title }" v-model="title"  v-on:keyup.enter="addTodo" placeholder="Что бы такое запланировать?">
      <div class="btn btn-add" :class="{ active: title }" @click.prevent="addTodo">+</div>
    </form>
  </div>
</template>

<script>
export default {
  name: 'AddTodo',
  data() {
    return {
      title: ''
    }
  },
  watch: {
    title() {
      this.$emit('title-changed', this.title)
    }
  },
  methods: {
    addTodo() {
      if (this.title.trim()) {
        const todo = {
          id: Date.now(),
          title: this.title,
          description: 'подробности...',
          completed: false,
          folder: ''
        }

        this.$emit('add-todo', todo)
      }

      this.title = ''
    }
  }
}
</script>

<style scoped>
form {
  display: flex;
  width: 100%;
  height: 40px;
}

input {
  border-radius: 3px;
  padding: 0.2em 1em;
  font-size: 1em;
}

.input-todo {
  margin-right: 8px;
  min-width: 150px;
  border: 1px solid #ddd; 
  flex: 1;
  transition: all ease-in 0.25s;
}

.input-todo:focus{
  outline: none;
  border: 1px solid #a3b1ff;
}

.input-todo::placeholder{
  color: rgba(0,0,0,0.3);
  font-style: italic;
}
</style>