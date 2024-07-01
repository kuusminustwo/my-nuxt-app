<template>
  <div>
    <h2>Login</h2>
    <form @submit.prevent="login">
      <label for="username">Username:</label>
      <input type="text" id="username" v-model="username" required>
      <label for="password">Password:</label>
      <input type="password" id="password" v-model="password" required>
      <button type="submit">Login</button>
    </form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      username: '',
      password: ''
    }
  },
  methods: {
    async login () {
      try {
        const response = await this.$axios.$post('/login', {
          username: this.username,
          password: this.password
        })
        // Handle successful login (e.g., store token in localStorage, redirect)
        console.log('Login successful:', response)
        // Example: Store token in localStorage (replace with your actual token handling)
        localStorage.setItem('token', response.data.token)
        console.log(response.data.token)
        // Redirect to another page after successful login
        this.$router.push('/profile')
      } catch (error) {
        console.error('Error logging in:', error)
        // Handle login error (show error message, reset form, etc.)
      }
    }
  }
}
</script>
