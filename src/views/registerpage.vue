<template>
    <div>
      <h1>Register Page</h1>
      <form @submit.prevent="register">
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
        <button type="submit">Register</button>
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
      async register() {
        try {
          const response = await axios.post('http://localhost:3333/user/register', {
            username: this.username,
            password: this.password
          });
  
          if (response.status === 200) {
            console.log(response.data.message);
            // Display a success message to the user
  
            // Redirect the user to the login page or another page as needed
            this.$router.push('/login');
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
  