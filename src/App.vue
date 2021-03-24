<template>
  <div id="app">
    <div class="container">

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
          <label>Age</label>
          <input type="text" class="form-control" v-model="newUser.age">
          <div class="text-danger" v-if="errors.users.age">
            <p>{{errors.users.age}}</p>
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
        <div class="form-group">
          <button class="btn btn-success" v-on:click="age()" >Age</button>
        </div>
      </div>
      <div>
        <table class="table table-dark table-border table-hover">
          <tr>
            <th>Name</th>
            <th>Surname</th>
            <th>Gender</th>
            <th>Age</th>
            <th>Action</th>
          </tr>
          <tr v-for="user in users" :key="user.id">
            <td>{{user.name}}</td>
            <td>{{user.surname}}</td>
            <td>{{user.gender}}</td>
            <td >{{user.age}} {{user.age > 50 ? "old" : "young"}}</td>
            <td>
              <button class="btn btn-danger" v-on:click="deleteUser(user.id)">Delete</button>
            </td>
            <td>
              <button class="btn btn-danger" v-on:click="changeItems(user.id) " >Change Item</button>
            </td>
          </tr>

        </table>
        <div id="div" style="display:none">
            <div class="form-group">
              <label >Name</label>
              <input type="text" class="form-control" v-model="changeUser.name">
            </div>
          <div class="form-group">
            <label >Surname</label>
            <input type="text" class="form-control" v-model="changeUser.surname">
          </div>
          <div>
            <button v-on:click="chItems" class="btn btn-success">Change User</button>
          </div>
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

      users:[],


      newUser:{
        name: "",
        surname:"",
        gender:"",
        age:""
      },
      changeUser:{
        name:"",
        surname:"",
        click :0,
        id:0
      },
      errors:{
        users:{
          name:"",
          surname:"",
          gender:"",
          age:""
        }
      }

    }
  },

  mounted(){
    this.getUsersStorage();
  },

  methods:{

    deleteUser(id){

      let users = localStorage.getItem("users",users) || null;
      users = JSON.parse(users);
      for(let i=0;i<users.length;i++){
        if(users[i].id == id){
          users.splice(users[i],1)
        }
      }
      users = JSON.stringify(users)
      localStorage.setItem('users',users)
      location.reload()

    },

    chItems(){
      let id = this.changeUser.id
      if(this.changeUser.name) {
        let users = localStorage.getItem("users", users) || null;
        users = JSON.parse(users);
        for (let i = 0; i < users.length; i++) {
          if (users[i].id == id) {
            users[i].name = this.changeUser.name
            users[i].surname = this.changeUser.surname
          }
        }
        users = JSON.stringify(users)
        localStorage.setItem('users', users)
        location.reload()
      }
    },

    changeItems(id)
      {
        this.changeUser.click++

        if (this.changeUser.click == 1) {
          $("#div").css({"display": "block"})
          this.changeUser.id = id
        }

        if (this.changeUser.click == 2) {
          $("#div").css({"display": "none"})
          this.changeUser.click = 0
        }


    },
    saveUser(){
      this.errors.users.name = this.newUser.name == "" ? "Name is required" : ""
      this.errors.users.surname = this.newUser.surname == "" ? "Surname is required" : ""
      this.errors.users.gender = this.newUser.gender == "" ? "Gender is required" : ""
      this.errors.users.age = this.newUser.age == "" ? "Agea is required" : ""

      if(this.errors.users.gender == "" && this.errors.users.name == "" && this.errors.users.surname == "" && this.errors.users.age == ""){
        const user = {};

        user.name = this.newUser.name;
        user.surname = this.newUser.surname;
        user.age = this.newUser.age;
        user.gender = this.newUser.gender;


        user.id = this.generateRandomNumber();
        this.users.push(user)
        this.setUsersStorage();
      }
    },
    setUsersStorage(){
        const users = JSON.stringify(this.users);
        localStorage.setItem("users",users);
    },
    getUsersStorage(){
      let users = localStorage.getItem("users",users) || null;
      users = JSON.parse(users);
      this.users = users ? users : [];

    },

    generateRandomNumber(){
      const d = new Date();
      return d.getTime() + Math.floor(Math.random() * 10);
    },

    age(){
      let rand = Math.ceil(Math.random() * 10)
      this.newUser.age = rand
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
