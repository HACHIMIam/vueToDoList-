<template>
<div>
<div class="container">
  <div class="row">
    <div class="col-md-8">
      <h1>Todo List</h1>
    </div>
    <div class="col-md-3">
      <button v-b-modal.addItem type="button" class="btn btn-primary">Add Elemet</button>
    </div>
  </div>
  <div class="row">
     <div v-for="(item,index) in list" v-bind:key="index" class="card w-100">
      <div class="card-body">
        <div class="row">
          <div class="col-md-10">
            <h5 class="card-title">{{item.title}}</h5>
            <p class="card-text">{{item.description}}</p>
            <button type="button" v-on:click="Editing(item,index)" v-b-modal.addItem class="btn btn-primary">edit</button>
            <button type="button" v-on:click="deleteItem(index)"  class="btn btn-danger">delete</button>
          </div>
          <div class="col-md-2">
             <input class="checkbox" type="checkbox" v-on:click="checkItem(index)"  v-model="item.checked">
          </div>
          </div>
      </div>
    </div>
  </div>
</div>
  <b-modal id="addItem" ref="modal"  @cancel="cancel" @ok="handleAdd" @shown="openModal" :ok-disabled="!this.title"  ok-title="save">
    <form @submit.stop.prevent="handleSubmit">
      <label for="title">Title:</label>
      <b-form-input name="title" type="text" placeholder="Title" v-model="title"></b-form-input>
      <label for="description">Description:</label>
      <b-form-input name="description" type="text" placeholder="Description" v-model="description"></b-form-input>
    </form>
  </b-modal>
</div>
</template>

<script>
export default {
  name: 'ToDolist',
  data () {
    return {
      editing : false,
      list : [{title:'task1',description:" Lorem ipsum dolor sit amet consectetur adipisicing elit. Odio asperiores blanditiis neque nisi, architecto accusantium! Saepe tempore ad porro",checked:false},{title:'task2',description:" Lorem ipsum dolor sit amet consectetur adipisicing elit. Odio asperiores blanditiis neque nisi, architecto accusantium! Saepe tempore ad porro",checked:true},{title:'task3',description:" Lorem ipsum dolor sit amet consectetur adipisicing elit. Odio asperiores blanditiis neque nisi, architecto accusantium! Saepe tempore ad porro",checked:true}],
      title:'',
      description:'',
      selectedItem:null,
    }
  },
  methods:{
    openModal(){
      if(!this.editing){
      this.title = ''
      this.description = ''
      }
    },
    cancel(){
      this.title = ''
      this.description = ''
    },
    handleAdd(){
        if(this.editing){
          this.list[this.selectedItem]={title:this.title,description:this.description}
          this.$forceUpdate()
        }
        else{
          this.list.push({title:this.title,description:this.description,checked:false})
        }
        this.editing=false
        this.selectedItem=null
        this.$refs.modal.hide()
    },
    deleteItem(index){
      this.list.splice(index,1)
    },
    Editing(item,index){
      this.editing = true
      this.selectedItem = index
      this.title = item.title
      this.description = item.description
    },
    checkItem(index){
      this.list[index].checked=!this.list[index].checked
    }

  }
}
</script>

<style scoped>
.card-body{
  text-align: left
}
form{
    text-align: left

}
form label {
  margin-top: 5px
}
.checkbox{
  width: 50px;
  height: 100px;
}
</style>
