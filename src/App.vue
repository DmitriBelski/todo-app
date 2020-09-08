<template>
  <div id="app">
    <h1>Todo Application</h1>

    <Popup 
      v-if="isInfoPopupVisible"
      @close-popup = "closePopupInfo"
      @submit-popup = "removeTodo"
    >
      <p style="padding: 0 10px 10px 10px; margin: 0 10px 10px 10px">Вы уверены, что хотите удалить задачу вместе с ее описанием?</p>
    </Popup>

    <div class="wrapper">
      <TodoFolder 
        @folder-selected = "folderSelected"
      />

      <section class="todo-wrapper">
        <TodoList 
          :todos = folderedTodos
          @remove-todo = "showPopupInfo"
          @add-todo = "addTodo"
          @title-changed = "titleChanged"
          @set-filter = "setFilter"
          @remove-completed = "removeCompleted"
          @array-of-selected = "selectedArray"
        />
      </section>
    </div>

  </div>  
</template>

<script>
import TodoFolder from '@/components/TodoFolder.vue'
import TodoList from '@/components/TodoList.vue'
import Popup from '@/components/Popup.vue'

export default {
  name: 'App',
  components: {
    TodoFolder, TodoList, Popup
  },
  data() {
    return {
      todos: [
        {
          id: 1,
          title: 'Помыть кота',
          description: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Eveniet rem hic voluptatibus quaerat, error reprehenderit est at possimus! Perferendis eveniet enim porro fuga sed veritatis, quos voluptatum nihil voluptates dolores dicta nemo laboriosam? Eveniet, repellat quis? Ipsa veniam optio, eum error, rerum iste nostrum officiis, blanditiis explicabo eos eveniet fugit?',
          completed: true,
          folder: 'Покупки'
        },
        {
          id: 2,
          title: 'Вытряхнуть коврик из большой комнаты, под журнальным столиком',
          description: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Eveniet rem hic voluptatibus quaerat, error reprehenderit est at possimus! Perferendis eveniet enim porro fuga sed veritatis, quos voluptatum nihil voluptates dolores dicta nemo laboriosam? Eveniet, repellat quis? Ipsa veniam optio, eum error, rerum iste nostrum officiis, blanditiis explicabo eos eveniet fugit?',
          completed: false,
          folder: 'Учеба'
        } ,
        {
          id: 3,
          title: 'Прогуляться в парке',
          description: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Eveniet rem hic voluptatibus quaerat, error reprehenderit est at possimus! Perferendis eveniet enim porro fuga sed veritatis, quos voluptatum nihil voluptates dolores dicta nemo laboriosam? Eveniet, repellat quis? Ipsa veniam optio, eum error, rerum iste nostrum officiis, blanditiis explicabo eos eveniet fugit?',
          completed: false,
          folder: 'Учеба'
        },
        {
          id: 4,
          title: 'Съесть арбуз',
          description: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Eveniet rem hic voluptatibus quaerat, error reprehenderit est at possimus! Perferendis eveniet enim porro fuga sed veritatis, quos voluptatum nihil voluptates dolores dicta nemo laboriosam? Eveniet, repellat quis? Ipsa veniam optio, eum error, rerum iste nostrum officiis, blanditiis explicabo eos eveniet fugit?',
          completed: false,
          folder: '',
          idToRemove: null
        },
        {
          id: 5,
          title: 'Пройти курс Vue.Js за 60 минут',
          description: 'Vue.js — JavaScript-фреймворк с открытым исходным кодом для создания пользовательских интерфейсов[6]. Легко интегрируется в проекты с использованием других JavaScript-библиотек. Может функционировать как веб-фреймворк для разработки одностраничных приложений в реактивном стиле.',
          completed: false,
          folder: 'Учеба',
          idToRemove: null
        },
        {
          id: 6,
          title: 'Написать первое ToDo приложение на Angular',
          description: 'AngularJS — JavaScript-фреймворк с открытым исходным кодом. Предназначен для разработки одностраничных приложений[5]. Его цель — расширение браузерных приложений на основе MVC-шаблона, а также упрощение тестирования и разработки. Фреймворк работает с HTML, содержащим дополнительные пользовательские атрибуты, которые описываются директивами, и связывает ввод или вывод области страницы с моделью, представляющей собой обычные переменные JavaScript. Значения этих переменных задаются вручную или извлекаются из статических или динамических JSON-данных.',
          completed: false,
          folder: 'Учеба',
          idToRemove: null
        },
        {
          id: 7,
          title: 'Прочитать книгу React с нуля до героя',
          description: 'React (иногда React.js или ReactJS) — JavaScript-библиотека[5] с открытым исходным кодом для разработки пользовательских интерфейсов. React разрабатывается и поддерживается Facebook, Instagram и сообществом отдельных разработчиков и корпораций[6][7][8]. React может использоваться для разработки одностраничных и мобильных приложений. Его цель — предоставить высокую скорость, простоту и масштабируемость. В качестве библиотеки для разработки пользовательских интерфейсов React часто используется с другими библиотеками, такими как MobX, Redux и GraphQL .',
          completed: false,
          folder: 'Учеба',
          idToRemove: null
        }
      ],
      filteredtodos: [],
      filter: 'all',
      search: '',
      selectedFolder: '',
      idsOfSelected: [],
      isInfoPopupVisible: false
    }
  },
  computed: {
    searchedTodos() {
      if (this.search) {
        let cloned_todos = this.todos.map(t => {
          return {...t, searchIndex: t.title.toUpperCase().indexOf(this.search.toUpperCase())}
        })
        let searched_todos = cloned_todos
            .slice()
            .sort((a, b) => a.searchIndex - b.searchIndex)
            .filter(t => t.searchIndex !== -1)
        return searched_todos
      } else {
        return this.todos
      }
    },
    filteredTodos() {
      let filtered_todos
      if (this.filter === 'all') {
        filtered_todos = this.searchedTodos
      } 
      if (this.filter === 'completed') {
        filtered_todos = this.searchedTodos.filter(t => t.completed)
      } 
      if (this.filter === 'active') {
        filtered_todos = this.searchedTodos.filter(t => !t.completed)
      }
      return filtered_todos
    },
    folderedTodos() {
      // todo фильтруется перед выводом на экран
      return this.filteredTodos.filter(f => f.folder === this.selectedFolder)
    }
  },
  watch: {
    selectedFolder(folder) {
      // когда папка меняется - здесь в нее запускаются таски по индексам из массива this.idsOfSelected
      // тоесть если this.idsOfSelected не пустой то файлы перекидываются а папка не открывается, просто снимается выделение
      if (this.idsOfSelected.length > 0) {
        let previousFolder
        for (let i=0;i<this.idsOfSelected.length;i++) {
          const id = this.idsOfSelected[i]
          const index = this.todos.findIndex(t => t.id === id)
          previousFolder = this.todos[index].folder
          this.todos[index].folder = folder
        }
        this.idsOfSelected.length = 0
        this.folderSelected(previousFolder)
        this.$emit('select-folder', previousFolder)
      }
    }
  },
  methods: {
    removeTodo() {
      this.todos = this.todos.filter(t => t.id !== this.idToRemove)
      this.closePopupInfo()
    },
    removeCompleted() {
      this.todos = this.todos.filter(t => !t.completed)
    },
    addTodo(todo) {
      this.todos.push({...todo, folder: this.selectedFolder})
    },
    titleChanged(title) {
      this.search = title
    },
    setFilter(value) {
      this.filter = value
    },
    folderSelected(folder) {
      // массив arrSelected в TodoList'e стирается
      // чтобы стереть this.idsOfSelected, надо заемитить сюда array-of-selected = [] из TodoList'а
      // снимаются стили выделения у todo в TodoItem
      // to Children - TodoList слушает 'clear-selected'
      if (this.idsOfSelected.length <= 0) {
        this.$emit('clear-selected', true)
      }
      // выбрали папку
      this.selectedFolder = folder
    },
    selectedArray(array) {
      this.idsOfSelected = [...array]
    },
    showPopupInfo(id) {
      this.idToRemove = id
      this.isInfoPopupVisible = true
    },
    closePopupInfo() {
      this.isInfoPopupVisible = false
    }
  }
}
</script>

<style>
*{
  box-sizing: border-box;
}

body, textarea, input {
  font-family: 'Open Sans', sans-serif;
}

body {
  font-size: 1em;
  color: #444;
  background-color: #fefefe;
  background-image: linear-gradient(#9046cf 0%, #cc59d2 100%);
  padding: 50px 20px;
  margin: 0;
  min-height: 100vh;
  position: relative;
}

h1{
  font-size: 2.7rem;
  color: #fff;
  text-align: center;
  margin-top: 0;
  margin-bottom: 30px;
}

@media only screen and (min-width: 813px) 
{
  .wrapper {
    align-items: flex-start;
  }
  .folder-wrapper {
    margin-right: 20px; 
    flex-direction: column;
  }
  .folder-btn  {
    margin-bottom: 8px;
  }
}

@media only screen and (max-width: 812px) 
{
  .wrapper {
    flex-direction: column;
  }
  .folder-wrapper {
    min-width: 400px;
    margin-bottom: 20px;
    /* display: flex; */
    justify-content: space-between;
    flex-wrap: wrap;
  }
  .folder,.folder-btn {
    width: 49%;
    height: auto;
    min-height: 10px;
  }
  .folder-btn {
    margin: 5px 0;
  }
}

.wrapper {
  display: flex;
  max-width: 1000px;
  margin: 0 auto;
}

.folder-wrapper {
  padding: 20px;
  flex: 2;
  box-shadow: 0 0 15px 0 rgba(0,0,0,0.1);
  background-color: #f4f7fc;
  border-radius: 4px;
  display: flex;
}

.todo-wrapper {
  flex: 5;
  min-width: 400px;
  border: 1px solid #eee;
  border-radius: 4px;
  padding: 20px 20px;
  -webkit-box-shadow: 0 0 15px 0 rgba(0,0,0,0.1);
  box-shadow: 0 0 15px 0 rgba(0,0,0,0.1);
  background-color: #f4f7fc;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.btn{
  text-align: center;
  font-weight: bold; 
  cursor: pointer;
  border-width: 1px;
  border-style: solid;  
  border-radius: 3px;
  padding: 0.2em 1em;
  font-size: 1em;
}

.btn:focus {
  outline-style: none;
  outline-width: 0px !important;
  outline-color: none !important;
}

.btn-add {
  background-color: #ddd;
  color: #fefefe;
  border-color: #ddd;
  min-width: 40px;
  /* pointer-events: none; */
  transition: background-color ease-in 0.25s;
  font-size: 2.2em;
  line-height: 0.5em;
  padding: 0.3em 0.3em;
  display: flex;
  align-items: center;
  justify-content: center;
}

.btn-add:hover {
  background-color: #4442f6;
  cursor: pointer;
}
</style>

