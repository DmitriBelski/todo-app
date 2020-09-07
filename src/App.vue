<template>
  <div id="app">
    <h1>Todo Application</h1>

    <div class="wrapper">

      <section class="folder-wrapper">
        <div class="btn folder-btn btn-add">+</div>
        <div class="folder">cd..</div>
        <div class="folder selected">
          Покупки
          <span class="delete"></span>
        </div>
        <div class="folder">Учеба</div>
        <div class="folder">Работа</div>
        <div class="folder">Домашние дела, мытье полов, посуды, стирка</div>
        <div class="folder">Здоровье</div>
        <div class="folder">Спорт</div>
        <div class="folder">Виолончель</div>
        <div class="folder">Китайский язык</div>
        <div class="folder">
          <input class="edit-folder" type="text">
        </div>
      </section>

      <section class="todo-wrapper">
        <form>
          <input type="text" class="input-todo" placeholder="Что бы такое запланировать?">
          <div class="btn btn-add">+</div>
        </form>

        <ul>
          <li class="selected">
            <input class="todo-checkbox" :id="1" type="checkbox">
            <label :for="1"></label>
            <span class="todo-text">
              <p class="title">Помыть кота</p>
              <p class="description">Lorem ipsum dolor sit amet consectetur adipisicing elit. Eveniet rem hic voluptatibus quaerat, error reprehenderit est at possimus! Perferendis eveniet enim porro fuga sed veritatis, quos voluptatum nihil voluptates dolores dicta nemo laboriosam? Eveniet, repellat quis? Ipsa veniam optio, eum error, rerum iste nostrum officiis, blanditiis explicabo eos eveniet fugit?</p>
            </span>
            <span class="delete"></span>
          </li>
          <li>
            <input class="todo-checkbox" :id="2" type="checkbox">
            <label :for="2"></label>
            <span class="todo-text">
              <p class="title">Вытряхнуть коврик из большой комнаты, под журнальным столиком</p>
              <p class="description open">Lorem ipsum dolor sit amet consectetur adipisicing elit. Eveniet rem hic voluptatibus quaerat, error reprehenderit est at possimus! Perferendis eveniet enim porro fuga sed veritatis, quos voluptatum nihil voluptates dolores dicta nemo laboriosam? Eveniet, repellat quis? Ipsa veniam optio, eum error, rerum iste nostrum officiis, blanditiis explicabo eos eveniet fugit?</p>
            </span>
            <span class="delete"></span>
          </li>
          <li>
            <input class="todo-checkbox" :id="3" type="checkbox">
            <label :for="3"></label>

            <form class="edit">
              <input class="edit-title" type="text">
              <textarea class="edit-description" rows="4" type="text"></textarea>
            </form>

            <span class="delete"></span>
          </li>
        </ul>

        <div class="todo-footer">
          <div class="items_left">6 items left</div>
          <div class="control-buttons">
            <div class="btn btn-secondary">All</div>
            <div class="btn btn-secondary">Active</div>
            <div class="btn btn-secondary">Completed</div>
          </div>
          <div class="btn btn-secondary clear">Clear Completed</div>
        </div>
      </section>
  
    </div>
  </div>  
</template>

<script>

export default {
  name: 'App',
  components: {
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
    display: flex;
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

.folder {
  position: relative;
  background-color: #fcab1035;
  font-weight: bold;
  border-radius: 3px;
  padding: 12px 55px 12px 12px;
  margin: 5px 0;
  cursor: pointer;
}

.folder:hover {
  background-color: #fcab1045;
}

.folder.selected {
  background-color: #fcab1065;
}

.edit-folder {
  color: #444;
  font-weight: bold;
  border: none;
  background: transparent;
  padding: 0;
  width: 100%;
}

.edit-folder:focus {
  outline: none;
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

form {
  display: flex;
  width: 100%;
  height: 40px;
}

.btn, input {
  border-radius: 3px;
  padding: 0.2em 1em;
  font-size: 1em;
}

.input-todo {
  margin-right: 8px;
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

.btn{
  text-align: center;
  font-weight: bold; 
  cursor: pointer;
  border-width: 1px;
  border-style: solid;
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

.todo-footer {
  margin-top: auto;
  display: flex;
  font-size: 0.9  rem;
  align-items: center;
  justify-content: space-between;
  padding-top: 5px;
  border-top: 1px solid #ccc;
}

.items_left {
  font-weight: bold; 
}

.control-buttons {
  display: flex;
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

ul {
  list-style: none;
  padding: 0;
}

li {
  margin-bottom: 8px;
  padding: 12px;
  border-radius: 3px;
  background: rgba(144, 70, 207, 0.1);
  display: flex;
  align-items: center;
  position: relative;
  cursor: pointer;
}

li:last-child {
  margin-bottom: 0;
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

.todo-checkbox{
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

.todo-checkbox:checked + label:after{
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

.todo-checkbox:checked ~ .todo-text{
  opacity: 0.3;
  text-decoration: line-through
}

.delete{
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

.delete:after{
  position: absolute;
  content: '';
  width: 16px;
  height: 16px;
  top: 50%;
  left: 50%;
  background-image: url(./assets/trash.svg);
  background-repeat: no-repeat;
  background-position: center;
  background-size: contain;
  transform: translate(-50%, -50%) scale(0.5);
  transition: all ease-in 0.25s;
}

li:hover .delete, .folder:hover .delete{
  width: 44px;
  opacity: 1;
}

li:hover .delete:after, .folder:hover .delete:after{
  transform: translate(-50%, -50%) scale(1);
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
