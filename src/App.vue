<template>
  <div id="app">
    <div class="container">
      <div v-if="user.name" v-bind:class="user.gender == 'male' ? 'male' : 'female'">
        <p>name:{{ user.name }}</p>
        <p>username: {{ user.surname }}</p>
        <p>gender:{{ user.gender }}</p>
      </div>


      <div>
        <div class="form-group">
          <label>Name</label>
          <input type="text" class="form-control" v-model="newUser.name">
          <div class="text-danger" v-if="errors.users.name">
            <p>{{errors.users.name}}</p>
          </div>
        </div>
        <div class="form-group">
          <label>Surname</label>
          <input type="text" class="form-control" v-model="newUser.surname">
          <div class="text-danger" v-if="errors.users.surname">
            <p>{{errors.users.surname}}</p>
          </div>
        </div>
        <div class="form-group">
          <label>Gender</label>
          <input type="text" class="form-control" v-model="newUser.gender">
          <div class="text-danger" v-if="errors.users.gender">
            <p>{{errors.users.gender}}</p>
          </div>
        </div>
        <div class="form-group">
          <button class="btn btn-success" v-on:click="saveUser">Change</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>


export default {
  name: 'App',
  data(){
    return {
      user:{
        name: "",
        surname:"",
        gender:""
      },

      newUser:{
        name: "",
        surname:"",
        gender:""
      },
      errors:{
        users:{
          name:"",
          surname:"",
          gender:""
        }
      }

    }
  },
  methods:{
    saveUser(){
      this.errors.users.name = this.newUser.name == "" ? "Name is required" : ""
      this.errors.users.surname = this.newUser.surname == "" ? "Surname is required" : ""
      this.errors.users.gender = this.newUser.gender == "" ? "Gender is required" : ""


      if(this.errors.users.gender == "" && this.errors.users.name == "" && this.errors.users.surname == ""){
        this.user.name = this.newUser.name
        this.user.surname = this.newUser.surname
        this.user.gender = this.newUser.gender
      }
    }

  }

}
</script>


<style>
.male{
  background: blue;
}

.female{
  background: lightpink;
}
</style>
