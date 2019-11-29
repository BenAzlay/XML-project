<template>
  <div id="login">
    <h1>Login</h1>
    <input
      type="text"
      name="username"
      v-model="input.username"
      placeholder="Username"
    />
    <input
      type="password"
      name="password"
      v-model="input.password"
      placeholder="Password"
    />
    <button type="button" v-on:click="login()">Login</button><br/>
    <p>Don't have an account yet? <router-link to="/sign-up">Sign Up</router-link></p>
  </div>
  
</template>

<script>
import { APIService } from "../APIService";

const apiService = new APIService();
export default {
  name: "Login",
  data() {
    return {
      input: {
        username: "",
        password: ""
      },
      users: []
    };
  },
  methods: {
    login() {
      if (this.input.username != "" && this.input.password != "") { //IF credentials are entered
        this.users.forEach((user) => {
            if ( 
                this.input.username == user.username &&
                this.input.password == user.password
            ) {
                this.$emit("authenticated", true);
                this.$parent.currentUser = user;
                this.$router.replace({ name: "home" });
            }
            else {
                console.log("The username and / or password is incorrect");
            }
        })
      } else {
        console.log("A username and password must be present");
      }
    },
    getUsers() {
      apiService.getUsers().then(data => {
        this.users = data;
      });
    }
  },
  mounted() {
    this.getUsers();
  }
};
</script>

<style scoped>
#login {
  width: 500px;
  border: 1px solid #cccccc;
  background-color: #ffffff;
  margin: auto;
  margin-top: 200px;
  padding: 20px;
}
</style>
