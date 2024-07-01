<template>
  <div v-if="user">
    <h2>Profile</h2>
    <p>Username: {{ user.username }}</p>
    <p>Email: {{ user.email }}</p>
    <p>Token: {{ token }}</p>
  </div>
  <div v-else>
    <p>Loading...</p>
  </div>
</template>

<script>
export default {
  data () {
    return {
      user: null,
      token: ''
    }
  },
  async created () {
    try {
      // Fetch user profile from backend API
      const token = localStorage.getItem('token')
      if (!token) {
        // Handle case where token is not available (redirect to login or handle error)
        throw new Error('No token available')
      }
      console.log('token' + token)

      const response = await this.$axios.$get('/profile', {
        headers: {
          Authorization: `Bearer ${token}`
        }
      })

      // Store user information and token
      this.user = response.data
      this.token = token
    } catch (error) {
      console.error('Error fetching user profile:', error)
      // Handle specific errors (e.g., redirect to login for 401 Unauthorized)
      if (error.response && error.response.status === 401) {
        // Redirect or handle unauthorized access
        // Example redirect: this.$router.push('/login');
      } else {
        // Handle other errors
      }
    }
  }
}
</script>
