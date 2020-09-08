<template>
  <section class="folder-wrapper">

    <Popup 
      v-if="isInfoPopupVisible"
      @close-popup = "closePopupInfo"
      @submit-popup = "submitRemove"
    >
      <p style="padding: 0 10px 10px 10px; margin: 0 10px 10px 10px">Вы уверены, что хотите удалить папку со всем ее содержимым?</p>
    </Popup>

    <div class="btn folder-btn btn-add" @click="addFolder">+</div>
    <div class="folder" @click="toggleSelected(-1)" :class="{selected: nothingSelected}">cd..</div>

    <div v-for="(folder, index) in folders" :key="index" class="folder" :class="{selected: folder.selected}" @click="clickHandler($event, index)">
      <div v-if="!folder.editing" class="folder-title" @dblclick="folder.editing=true, folder.selected=true">{{folder.title}}</div>
      <input v-else class="edit-folder" type="text" 
        :value="folder.title"
        v-focus="folder.editing"
        @blur="doneEdit($event, index)"
        @keyup.enter="doneEdit($event, index)"
        @keyup.esc="cancelEdit(index)"
      >
      <span class="delete"></span>
    </div>
  </section>
</template>

<script>
import Popup from './Popup.vue'

export default {
  name: 'TodoFolder',
  components: { Popup },
  data() {
    return {
      folders: [
        { title: 'Покупки', selected: false, editing: false },
        { title: 'Учеба', selected: false, editing: false },
        { title: 'Работа', selected: false, editing: false },
        { title: 'Домашние дела, мытье посуды, стирка', selected: false, editing: false },
        { title: 'Здоровье', selected: false, editing: false },
        { title: 'Спорт', selected: false, editing: false },
        { title: 'Виолончель', selected: false, editing: false },
        { title: 'Китайский язык', selected: false, editing: false }
      ],
      isInfoPopupVisible: false,
      indexToSubmit: null
    }
  },
  methods: {
    doneEdit(event, index) {
      const value = event.target.value.trim()
      if (!value) {
        this.removeFolder(index)
      } else if (this.folders[index].editing) {
        this.folders[index].title = value
        this.folders[index].editing = false
        this.folders[index].selected = false
      }
    },
    cancelEdit(index) {
      this.folders[index].editing = false
      this.folders[index].selected = false
    },
    removeFolder(index) {
      this.folders.splice(index, 1)
      this.$emit('folder-selected', this.nowSelected !== -1 ? this.folders[this.nowSelected].title : '')
    },
    addFolder() {
      this.folders.push({title: 'Новая папка', selected: false, editing: false})
    },
    toggleSelected(index) {
      const nowSelected = this.nowSelected
      this.folders.map(f => f.selected = false)
      if (index !== -1) {
        this.folders[index].selected = nowSelected !== index
      } 
      this.$emit('folder-selected', this.nowSelected !== -1 ? this.folders[this.nowSelected].title : '')
    },
    straightSelected(foldername) {
      const index = this.folders.findIndex  (f => f.title == foldername)
      this.toggleSelected(index)
    },
    clickHandler(e, index) {
      if (e.target.tagName === 'SPAN') {
        this.indexToSubmit = index
        this.showPopupInfo()
      } else {
        this.toggleSelected(index)
      }
    },
    showPopupInfo() {
      this.isInfoPopupVisible = true
    },
    submitRemove() {
      this.removeFolder(this.indexToSubmit)
      this.closePopupInfo()
    },
    closePopupInfo() {
      this.isInfoPopupVisible = false
    }
  },
  computed: {
    nothingSelected() {
      return this.folders.findIndex(f => f.selected === true) === -1
    },
    nowSelected() {
      return this.folders.findIndex(f => f.selected === true)
    }
  },
  created() {
    this.$parent.$on('select-folder', this.straightSelected)
  }
}
</script>

<style scoped>
.folder {
  position: relative;
  background-color: #fcab1035;
  font-weight: bold;
  box-shadow: 0 1px 1px 0 rgba(0,0,0,0.2);
  border-radius: 3px;
  padding: 12px 55px 12px 12px;
  margin: 5px 0;
  cursor: pointer;
  transition: all ease-in 0.25s;
}

.folder-title{
  cursor: text;
}

.folder:hover {
  background-color: #fcab1045;
}

.folder.selected {
  background-color: #fcab1065;
}

.edit-folder {
  color: #444;
  font-size: 1rem;
  font-weight: bold;
  border: none;
  background: transparent;
  padding: 0;
  width: 100%;
}

.edit-folder:focus {
  outline: none;
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
</style>