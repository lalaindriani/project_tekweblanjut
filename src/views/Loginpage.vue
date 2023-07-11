<template>
    <div>
      <h1>Login Page</h1>
      <form @submit.prevent="login">
        <label>
          Username:
          <input type="text" v-model="username" required>
        </label>
        <br>
        <label>
          Password:
          <input type="password" v-model="password" required>
        </label>
        <br>
        <button type="submit">Login</button>
      </form>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        username: '',
        password: ''
      };
    },
    methods: {
      async login() {
        try {
          const response = await axios.post('http://localhost:3333/user/login', {
            username: this.username,
            password: this.password
          });
  
          if (response.status === 200) {
            const { token } = response.data;
            // Save the token to local storage as a cookie
            localStorage.setItem('token', token);
  
            // Redirect the user to another page (e.g., Dashboard page)
            this.$router.push('/rencana');
          } else {
            console.error(response.data.error);
            // Display an error message to the user
          }
        } catch (error) {
          console.error(error);
          // Display an error message to the user
        }
      }
    }
  };
  </script>
  <style scoped>
  
  </style>
  