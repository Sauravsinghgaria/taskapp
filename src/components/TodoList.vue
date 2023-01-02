<template>
  <div class="col-3 list-column list-width">
    <div class="heading" style="background-color: rgb(96, 125, 139);">
      <h4 class="heading-text">{{indexlist}} {{ listname }}</h4>
      <details class="detail-dropdown" style="position: absolute; top: 38px;right: 20px;">
        <summary>...</summary>
        <div class="dropdown-content">
          <label class="content-item">
            <details class="popup">
              <summary>
                <span class="nav-item btn btn-sm btn-app mr-2">Edit</span>
              </summary>
              <div>
                <form @submit.prevent>
                  <h4>Edit List Name</h4>
                  <input required name="listName" v-model="nameoflist" type="text" data-vv-as="List Name" placeholder="Enter your list name" class="form-control my-1" aria-required="true" aria-invalid="false">
                  <small class="text-danger" style="display: block;"></small>
                  <button class="btn btn-sm btn-app mt-2" @click="editname(indexlist,nameoflist)"> Save List </button>
                  <button class="btn btn-sm btn-app mt-2" @click="editname"> Cancel List </button>
                </form>
              </div>
            </details>
          </label>
          <label class="content-item">
            <details class="popup" style="">
              <summary>
                <span class="nav-item btn btn-sm btn-app mr-2">Delete</span>
              </summary>
              <div>
                <form @submit.prevent>
                  <h4>Delete List</h4>
                  <p><b>WARN: This list will be deleted.</b></p>
                  <button class="btn btn-sm btn-app mt-2" @click="deletename(indexlist)"> Delete List </button>
                </form>
              </div>
            </details>
          </label>
        </div>
      </details>
    </div>
    <div class="cards cards-list">
      <div v-for="(tx,index) in textdesc" :key="index">
        <div class="card tasklist-item">
          <div class="card-body">
            <div class="text-dark disable-select">
              <div v-if="tx.checked">
                <span @click="onchange(tx)">{{ tx.text }} </span>
              </div>
              <div v-else-if="!tx.checked">
                <form @submit.prevent>
                  <textarea placeholder="Your item description" required style="width:250px; height:75px;"
                            v-model="tx.text"></textarea>
                  <button @click="onchange(tx)">Save</button>
                  <button @click="onchange(tx)">Cancel</button>
                  <button @click="deleteItem(tx,index)" style="color: red; margin-right: 10px;">Delete</button>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="card tasklist-item fixed-card">
        <div class="card-body">
          <div class="text-center text-dark font-weight-bold disable-select">
            <div v-if="valuekey">
              <span @click="changevaluekey()"> + New Item </span>
            </div>
            <div v-else>
              <form @submit.prevent>
                <textarea placeholder="Your item description" v-model="text" required
                          style="width:250px; height:75px;"></textarea>
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
export default {
  name: 'TodoList',
  data() {
    return {
      valuekey: true,
      text: null,
      textdesc: [],
      checked: this.defaultChecked,
      listoftodo: this.arrList,
      nameoflist:this.listname,
    }
  },
  props: {
    listname: String,
    indexlist: Number,
    arrList: Array,
    defaultChecked: {
      type: Boolean, default: true
    }
  },
  methods: {
    editname(idx,name){
      // this.listoftodo.splice(idx,1,name)
      this.listoftodo[idx] = name
      console.log(this.listoftodo[idx])
      console.log(name)
    },
    deletename(idx){
      this.listoftodo.splice(idx,1)
    },
    deleteItem(tx,index) {
        this.textdesc.splice(index,1);
        tx.checked = !tx.checked
    },
    onchange(tx) {
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
    }
  }
}
</script>
<style>
.mt-1, .my-1 {
  margin-top: 0.25rem !important;
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
.heading {
  padding: 15px 0 5px 0;
  color: hsla(0, 0%, 100%, .8);
  font-family: Gugi, cursive;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  text-transform: uppercase;
  cursor: grab;
}
.heading-text {
  display: block;
  margin-block-start: 1.33em;
  margin-block-end: 1.33em;
  margin-inline-start: 0px;
  margin-inline-end: 0px;
  font-weight: bold;
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
  font-family: Arial;
  text-align: center;
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
.col-3 {
  flex: 0 0 25%;
  max-width: 25%;
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
  font-weight: 500;
  line-height: 1.2;
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
  font-size: 13px;
  background-color: hsla(0, 0%, 100%, .85);
  cursor: grab;
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