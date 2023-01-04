<template>
  <main>
    <nav>
      <h2>Kanban Board</h2>
      <div >
        <details class="popup" style="" >
          <summary>
            <span class="nav-item btn btn-sm btn-app mr-2">Archieved List</span>
          </summary>
          <div>
            <h4>Archieved List</h4>
            <ul class="mb-2 list-group">
              <li  v-for="(name,index) in restore" :key="index" class="list-group-item d-flex justify-content-between align-items-center">
                <p>{{name}}</p>
                <button class="btn btn-sm btn-app mt-2" style="color: #fff!important; background-color: #218838!important; border-color: #1e7e34!important;" @click="restorelist(name,index)"> Restore List </button>
              </li>
            </ul>
          </div>
        </details>
        <details class="popup" >
          <summary slot>
            <span class="nav-item btn btn-sm btn-app mr-2">+ New List</span>
          </summary>
          <div slot>
            <form @submit.prevent>
              <h4>Create new list</h4>
              <input required name="listName" v-model="listname" type="text" data-vv-as="List Name" placeholder="Enter your list name" class="form-control my-1" aria-required="true" aria-invalid="false">
              <small class="text-danger" style="display: block;"></small>
              <button class="btn btn-sm btn-app mt-2" @click="addList()"> Save List </button>
            </form>
          </div>
        </details>
      </div>
    </nav>
    <div class="scrolling-wrapper">
      <div class="row flex-nowrap mt-1">
          <div v-for="(list,index) in Listnames" :key="index" class="col-3 list-column list-width">
            <TodoList
                :listname= list.listname
                :indexlist = index
                :arrList = Listnames
                :updatearr = restore
            />
          </div>
      </div>
    </div>
  </main>
</template>
<script>

import TodoList from "@/components/TodoList.vue";

export default {
  name:'HeadingTodo',
  components: {TodoList,

  },
  data(){
    return{
      listname:null,
      Listnames:[
        {listname:"Todo"},
        {listname:"Done"},
        {listname:"Doing"},
        {listname:"Deploy"},
      ],
      restore:[],
    }
  },
  methods:{
    addList(){
      if(this.listname!=null && this.listname.trim().length !== 0){
        this.Listnames.push({listname: this.listname})
        this.listname= "";
      }
      console.log(this.Listnames[0].listname)
    },
    restorelist(name,index){
      this.Listnames.push({listname: name})
      this.restore.splice(index,1)

    },

  }
}


</script>
<style>
.mb-2 {
  margin-bottom: 0.5rem!important;
}

.list-group {
  display: flex;
  flex-direction: column;
  padding-left: 0;
  margin-bottom: 0;
  border-radius: 0.25rem;
}
.list-group-item:first-child {
  border-top-left-radius: inherit;
  border-top-right-radius: inherit;
}

.align-items-center {
  align-items: center!important;
}
.justify-content-between {
  justify-content: space-between!important;
}
.d-flex {
  display: flex!important;
}
.list-group-item {
  position: relative;
  display: block;
  padding: 0.75rem 1.25rem;
  background-color: #fff;
  border: 1px solid rgba(0,0,0,.125);
}
*, :after, :before {
  box-sizing: border-box;
}
li {
  display: list-item;
  text-align: -webkit-match-parent;
}
details.popup summary {
  outline: none;
  cursor: pointer;
  display: inline-block;
}
.btn:not(:disabled):not(.disabled) {
  cursor: grab;
}
details.popup div {
  position: fixed;
  top: 50%;
  left: 50%;
  animation: fadein .2s ease-in-out;
  transform: translate(-50%, -50%);
  max-height: calc(100vh - 80px);
  max-width: 600px;
  width: calc(100% - 80px);
  overflow-y: auto;
  z-index: 999;
  color: #000;
  background-color: #fff;
  padding: 20px;
  box-shadow: 1px 1px 10px rgb(0 0 0 / 30%);
  border-radius: 5px;
}
.btn-app {
  background-color: rgba(96,125,139,.8)!important;
  border-color: rgba(96,125,139,.8)!important;
  color: #fff!important;
  text-decoration: none!important;
}
.mr-2 {
  margin-right: 0.5rem!important;
}
.btn-sm {
  padding: 0.25rem 0.5rem;
  font-size: .875rem;
  line-height: 1.5;
  border-radius: 0.2rem;
}
.btn {
  display: inline-block;
  font-weight: 400;
  color: #212529;
  text-align: center;
  vertical-align: middle;
  user-select: none;
  background-color: transparent;
  border: 1px solid transparent;
  padding: 0.375rem 0.75rem;
  font-size: 1rem;
  line-height: 1.5;
  border-radius: 0.25rem;
  transition: color .15s ease-in-out,background-color .15s ease-in-out,border-color .15s ease-in-out,box-shadow .15s ease-in-out;
}
*{

  font-style: normal;
}
nav{
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
}
button{
  margin-right:30px;
  font-weight: bolder;
  padding: 3px 10px;
  box-sizing: border-box;
}
h2{
  margin-left: 30px;
  font-family: Gugi, cursive;
  font-weight: bold;
}
.btn-sm {
  padding: 0.25rem 0.5rem;
  font-size: .875rem;
  line-height: 1.5;
  border-radius: 0.2rem;
}
details {
  display: inline-block;

}

.flex-nowrap {
  flex-wrap: nowrap !important;
}
.row {
  display: flex;
  flex-wrap: wrap;
  margin-right: -15px;
  margin-left: -15px;
}
.scrolling-wrapper {
  overflow-x: scroll;
  overflow-y: hidden;
  -webkit-overflow-scrolling: touch;
}
h4, h5 {
  margin-bottom: 0.5rem;
  font-weight: 350;
  line-height: 1.2;
}
input {
  width: 550px;
  margin-bottom: 15px;
  height: 35px;
  font-size: medium;
}
</style>