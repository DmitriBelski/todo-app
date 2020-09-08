<template>
  <div>

    <li :class="{selected: this.selected}" @click="toggleSelected">
      <input class="todo-checkbox" :id="index" type="checkbox" v-model="todo.completed">
      <label :for="index"></label>

      <span v-if="!editing" class="todo-text" :class="{edit: editing}">
        <p class="title" @dblclick="editingTitle = true" :class="{checked: todo.completed}">{{todo.title}}</p>
        <p class="description"
          :class="{open: descriptionOpen, checked: todo.completed}"
          @dblclick="editingDescription = true"
          @click="descriptionOpen = !descriptionOpen"
        >
          {{todo.description}}
        </p>
      </span>

      <span v-else-if="editingTitle" class="todo-text" :class="{edit: editing}">
        <input class="edit-title" type="text" 
          :value="todo.title" 
          v-focus="editing"
          @blur="doneEditTitle"
          @keyup.enter="doneEditTitle"
          @keyup.esc="cancelEditTitle">
        <p class="description"
          :class="{open: descriptionOpen, checked: todo.completed}"
          @click="descriptionOpen = !descriptionOpen"
        >
          {{todo.description}}
        </p>
      </span>

      <span v-else-if="editingDescription" class="todo-text" :class="{edit: editing}">
        <p class="title" :class="{checked: todo.completed}">{{todo.title}}</p>
        <textarea class="edit-description" rows="4" type="text" 
          :value="todo.description" 
          v-focus="editing"
          @blur="doneEditDescription"
          @keyup.enter="doneEditDescription"
          @keyup.esc="cancelEditDescription"
        ></textarea>
      </span>

      <span class="delete" @click="$emit('remove-todo', todo.id)"></span>
    </li>

  </div>
</template>

<script>
export default {
  name: 'TodoItem',
  props: {
    todo: {
      type: Object,
      required: true
    },
    index: Number
  },
  data() {
    return {
      editingTitle: false,
      editingDescription: false,
      descriptionOpen: false,
      selected: false
    }
  },
  computed: {
    editing() {
      return this.editingTitle || this.editingDescription
    }
  },
  methods: {
    doneEditTitle(e) {
      const value = e.target.value.trim()
      if (!value) {
        this.$emit('remove-todo', this.todo.id)
      } else if (this.editing) {
        this.todo.title = value
        this.editingTitle = false
      }
    },
    cancelEditTitle() {
      this.editingTitle = false
    },
    doneEditDescription(e) {
      const value = e.target.value.trim()
      if (this.editing) {
        this.todo.description = value
        this.editingDescription = false
      }
    },
    cancelEditDescription() {
      this.editingDescription = false
    },
    toggleSelected() {
      this.selected = !this.selected
      this.$emit('selection-toggled', this.todo.id, this.selected)
    },
    clearSelected() {
      this.selected = false
    }
  },
  created() {
    this.$parent.$on('clear-selected', this.clearSelected)
  }
}
</script>

<style scoped>
li {
  margin-bottom: 8px;
  padding: 12px;
  border-radius: 3px;
  box-shadow: 0 1px 1px 0 rgba(0,0,0,0.3);
  background: rgba(144, 70, 207, 0.1);
  display: flex;
  align-items: center;
  position: relative;
  cursor: pointer;
}

li:hover {
  background: rgba(144, 70, 207, 0.15);
}

li.selected {
  background: rgba(144, 70, 207, 0.25);
}

.todo-text{
  padding: 0 40px 0 12px;
  flex: 1;
  min-width: 0;
}

.title {
  margin: 0;
  margin-bottom: 0.5em;
  font-weight: bold;
  cursor: text;
  display: inline-block;
}

.description {
  margin: 0;
  padding: 0;
  font-size: 0.7em;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  cursor: text;
}

.description.open {
  white-space: normal;
}

.todo-checkbox {
  opacity: 0;
  display: none;
}

.todo-checkbox + label {
  cursor: pointer;
  width: 22px;
  height: 22px;
  border-radius: 2px;
  border: 1px solid #cfdcec;
  background-color: #fff;
}

.todo-checkbox:checked + label:after {
  position: absolute;
  content: '';
  margin-top: 4px;
  left: 24px;
  height: 4px;
  width: 8px;
  border: solid #4442f6;
  border-width: 0 0 2px 2px;
  transform-origin: center center;
  transform: rotate(-45deg) translate(-50%, -50%);
}

.checked {
  opacity: 0.45;
  text-decoration: line-through;
}

.delete {
  position: absolute;
  border-radius: 0 3px 3px 0;
  height: 100%;
  top: 50%;
  right: 0;
  transform: translateY(-50%);
  cursor: pointer;
  opacity: 0;
  width: 44px;
  background-color: #f56468;
  color: #fff;
  transition: all ease-in 0.25s;
}

.delete:after {
  position: absolute;
  content: '';
  width: 16px;
  height: 16px;
  top: 50%;
  left: 50%;
  background-image: url(../assets/trash.svg);
  background-repeat: no-repeat;
  background-position: center;
  background-size: contain;
  transform: translate(-50%, -50%) scale(0.5);
  transition: all ease-in 0.25s;
}

li:hover .delete, .folder:hover .delete {
  width: 44px;
  opacity: 1;
}

li:hover .delete:after, .folder:hover .delete:after {
  transform: translate(-50%, -50%) scale(1);
}

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

.edit {
  padding: 0 40px 0 12px;
  display: flex;
  flex-direction: column;
  height: auto;
}

.edit-title {
  color: #444;
  font-weight: bold;
  border: none;
  margin-bottom: 8px;
  background: transparent;
  padding: 0;
}

.edit-title:focus {
  outline: none;
}

.edit-description {
  font-size: 0.7em;
  color: #444;
  padding: 0;
  background: transparent;
  border: none; 
  resize: none;
  overflow: auto;
}

.edit-description:focus {
  outline: none;
}
</style>