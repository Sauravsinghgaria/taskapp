<template>
  <div class="list-column list-width">
      <div class="heading" style="background-color: rgb(96, 125, 139);">
        <h4 class="heading-text">{{listname}}</h4>
        <details class="detail-dropdown" style="position: absolute;top: 20px;right: 15px;font-size: 1.2rem;" >
          <summary>...</summary>
          <div class="dropdown-content">
            <label class="content-item">
              <details class="popup">
                <summary>
                  <span class="nav-item btn heading btn-sm btn-app mr-2">Edit</span>
                </summary>
                <div>
                  <form @submit.prevent>
                    <h4>Edit List Name</h4>
                    <input required name="listName" v-model="nameoflist" type="text" placeholder="Enter your list name" class="form-control my-1">
                    <button class="btn btn-sm btn-app mt-2" @click="editname()"> Save List </button>
                  </form>
                </div>
              </details>
            </label>
            <label class="content-item">
              <details class="popup">
                <summary>
                  <span class="nav-item heading btn btn-sm btn-app mr-2">Delete</span>
                </summary>
                <div>
                    <h4>Delete List</h4>
                    <p><b>WARN: This list : {{listname}} will be deleted.</b></p>
                    <button class="btn btn-sm btn-app mt-2" @click="deletename()"> Delete List </button>
                </div>
              </details>
            </label>
            <label class="content-item">
              <details class="popup">
                <summary>
                  <span class="nav-item  heading btn btn-sm btn-app mr-2">Archieve</span>
                </summary>
                <div>
                    <h4>Archieve List</h4>
                    <p><b>WARN: This list : {{listname}} will be Archieved but you can Restore.</b></p>
                    <button class="btn btn-sm btn-app mt-2 " @click="archievelist(indexlist)"> Archieve List </button>
                </div>
              </details>
            </label>
          </div>
        </details>
      </div>
      <div class="cards cards-list">
        <draggable group="my-group">
            <div class="card tasklist-item" v-for="(tx,index) in textdesc" :key="index">
              <div class="card-body">
                <div class="text-dark disable-select">
                  <div v-if="tx.checked" @click="onchange(tx)">
                    <span >{{ tx.text }} </span>
                  </div>
                  <div v-else-if="!tx.checked">
                    <form @submit.prevent>
                      <textarea placeholder="Your item description" style="width:250px; height:75px;"
                                v-model="tx.newtext" class="fixed-card"></textarea>
                      <button @click="editonchange(tx)">Save</button>
                      <button @click="cancelonchange(tx)">Cancel</button>
                      <button @click="deleteItem(tx,index)" style="color: red; margin-right: 10px;">Delete</button>
                    </form>
                  </div>
                </div>
              </div>
            </div>
          </draggable>
        <div class="card fixed-tasklist-item fixed-card" >
          <div class="card-body" >
            <div class="text-center text-dark font-weight-bold disable-select">
              <div v-if="valuekey" @click="changevaluekey()" class="item">
                <span> + New Item </span>
              </div>
              <div v-else>
                <form @submit.prevent>
                  <textarea placeholder="Your item description" v-model="text" required
                            style="width:250px; height:75px;" class="fixed-card"></textarea>
                  <button @click="addtask()">Save</button>
                  <button @click="changevaluekey()">Cancel</button>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
  </div>
</template>
<script>
import draggable from "vuedraggable";
export default {
  components:{
    draggable
  },
  name: 'TodoList',
  data() {
    return {
      newtext:this.text,
      valuekey: true,
      text: String,
      textdesc: [],
      checked: this.defaultChecked,
      listoftodo: this.arrList,
      nameoflist: this.listname,
      arcname :this.updatearr,
    }
  },
  props: {
    listname: String,
    indexlist: Number,
    arrList: Array,
    updatearr:Array,
    defaultChecked: {
      type: Boolean, default: true
    }
  },
  methods: {
    editname(){
      // console.log(this.arrList)
      this.listoftodo.splice(this.indexlist, 1, this.nameoflist)
      // console.log(this.arrList)

    },
    archievelist(){
      this.arcname.push(this.listname);
      this.listoftodo.splice(this.indexlist , 1)
    },
    deletename(){
      this.listoftodo.splice(this.indexlist , 1);
    },
    onchange(tx) {
      tx.checked = !tx.checked
    },
    deleteItem(tx,index){
        this.textdesc.splice(index,1);
        tx.checked = !tx.checked;
    },
    editonchange(tx){

      tx.text = ""
      tx.text = tx.newtext
      tx.checked =!tx.checked
    },
    cancelonchange(tx) {
      tx.newtext = tx.text
      tx.checked = !tx.checked
    },
    changevaluekey() {
      this.valuekey = !this.valuekey;
      this.text = '';
    },
    addtask() {
      if (this.text != null && this.text.trim().length !== 0) {
        this.textdesc.push({text: this.text, checked : this.checked});
        this.text = '';
        this.valuekey = !this.valuekey;
      }
      console.log(this.textdesc[this.indexlist].text)
    }
  }
}
</script>
<style>
.item{
  text-align: center;
  font-weight: 550;
}
button {
  margin-right: 1rem;
  padding: 5px 14px;
  border: 1px solid black;
  border-radius: 3px;
  box-sizing: border-box;
  background-color: white;
}
.fixed-card {
  cursor: pointer;
  color: #3e3939;
  border: 1px dotted #ccc;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
}

.card {
  position: relative;
  display: flex;
  flex-direction: column;
  min-width: 0;
  word-wrap: break-word;
  background-color: #fff;
  background-clip: border-box;
  border: 1px solid rgba(0,0,0,.125);
  border-radius: 0.25rem;
}
.mt-1, .my-1 {
  margin-top: 0.25rem !important;
}
.heading {
  color: #607d8b;
  padding: 15px 0 5px 0;
  color: hsla(0, 0%, 100%, .8);
  font-family: Gugi, cursive;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  text-transform: uppercase;
  cursor: grab;
}

details.detail-dropdown div.dropdown-content label.content-item {
  width: 100%;
}
label {
  display: inline-block;
  margin-bottom: 0.5rem;
}
*, :after, :before {
  box-sizing: border-box;
}
.heading-text {
  padding: 10px;
  font-family: Gugi,cursive;
  text-align: center;
  margin: 0;
  font-size: 1.4rem!important;
}
details {
  display: inline-block;
}
details.detail-dropdown summary {
  outline: none;
  cursor: pointer;
  display: inline-block;
}
.list-width {
  min-width: 300px;
  max-width: 300px;
  border-radius: 10px;
}
.list-column {
  padding: 0 !important;
  margin: 0 15px;
}
details.detail-dropdown div.dropdown-content {
  color: #607d8b;
  position: absolute;
  top: 25px;
  left: -40px;
  width: 100px;
  z-index: 99;
  background-color: #ecf5f8;
  cursor: pointer;
  padding: 5px;
  box-shadow: 1px 1px 1px hsl(0deg 0% 56% / 40%);
  border-radius: 5px;
}
h4, h5 {
  margin-bottom: 0.5rem;
  font-weight: 300;
  line-height: 1.2;
  text-transform: uppercase;
}
.cards-list {
  min-height: 300px;
  height: 100vh;
  overflow: scroll;
  box-shadow: 1px 1px 1px 0 hsl(0deg 0% 62% / 25%);
  background-color: rgba(223, 238, 242, .4);
}
.tasklist-item {
  min-height: 50px;
  border-bottom: 0.01rem solid rgba(0, 0, 0, .9);
  font-size: 14px;
  background-color: hsla(0, 0%, 100%, .85);
  cursor: grab;
  border-radius: 5px;
}
.fixed-tasklist-item {
  min-height: 50px;
  font-size: 14px;
  background-color: hsla(0, 0%, 100%, .85);
  border-radius: 5px;
}
.card-body {
  padding: 1rem !important;
}
.card-body {
  flex: 1 1 auto;
  min-height: 1px;
  padding: 1.25rem;
}
.col, .col-3, .col-md-3, .col-sm-6 {
  position: relative;
  width: 100%;
  padding-right: 15px;
  padding-left: 15px;
}
</style>