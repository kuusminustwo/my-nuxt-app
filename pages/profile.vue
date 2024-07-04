<template>
  <div class="container">
    <!-- Menu and other content -->
    <div class="circle-background">
      <div class="circle" v-for="n in 10" :key="n"></div>
    </div>

    <img src="../pic/logo2.png" alt="Logo" class="logo">

    <div v-if="user && student" class="profile">
      <div class="student-info">
        <h2>{{ student.lastname }} овогтой {{ student.firstname }}</h2>
        <p><strong>Сурагчийн ID:</strong> {{ student.student_id }}</p>
        <p><strong>Хөтөлбөр:</strong> {{ student.major }}</p>
        <p><strong>Email:</strong> {{ user.email }}</p>
      </div>
    </div>
    <div v-else>
      <p>Loading...</p>
    </div>

    <div class="button-group">
      <button @click="handleButtonClick('Тодорхойлолт')">Тодорхойлолт</button>
      <button @click="handleButtonClick('Дүнгийн хуудас')">Дүнгийн хуудас</button>
      <button @click="handleButtonClick('Хичээл сонголт')">Хичээл сонголт</button>
      <button @click="handleLogout" class="logout-button">Гарах</button>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      user: null,
      student: null,
      token: ''
    }
  },
  async created () {
    try {
      const token = localStorage.getItem('token')
      if (!token) {
        throw new Error('No token available')
      }

      const response = await this.$axios.$get('/profile', {
        headers: {
          Authorization: `Bearer ${token}`
        }
      })

      this.user = response.data.user
      this.student = response.data.student
      this.token = token
    } catch (error) {
      console.error('Error fetching user profile:', error)
      if (error.response && error.response.status === 401) {
        // Handle unauthorized access (e.g., redirect to login)
      } else {
        // Handle other errors
      }
    }
  },
  methods: {
    handleButtonClick (buttonType) {
      switch (buttonType) {
        case 'Тодорхойлолт':
          this.$router.push('/tod')
          break
        case 'Дүнгийн хуудас':
          this.$router.push('/dun')
          break
        case 'Хичээл сонголт':
          this.$router.push('/hicheel')
          break
        default:
          break
      }
    },
    handleLogout () {
    // Clear token from local storage
      localStorage.removeItem('token')

      // Redirect to login page
      this.$router.push('/')
    }
  }
}
</script>

<style scoped>
.container {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  text-align: center;
  overflow: hidden; /* To hide overflow of circles */
}

.profile {
  max-width: 600px;
  margin-bottom: 20px;
  border: 1px solid #ddd;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.profile h2 {
  margin-bottom: 10px;
}

.profile-info {
  margin-bottom: 20px;
}

.student-info {
  border-top: 1px solid #ddd;
  padding-top: 20px;
}

.button-group {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-top: 20px;
  width: 80%;
}

button {
  justify-content: center;
  align-items: center;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  background-color: #48C9B0;
  color: #fff;
  border: none;
  border-radius: 5px;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #399e92;
}

.logout-button {
  background-color: #e74c3c; /* Red color */
}

.logout-button:hover {
  background-color: #c0392b; /* Darker red on hover */
}

.circle-background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1; /* Ensure it is behind other content */
  overflow: hidden;
}

.circle {
  position: absolute;
  width: 20px;
  height: 20px;
  background-color: rgba(0, 255, 255, 0.2);
  border-radius: 50%;
  animation: float 8s infinite;
}

.circle:nth-child(2) {
  width: 30px;
  height: 30px;
  left: 25%;
  animation-duration: 8s;
}

.circle:nth-child(3) {
  width: 40px;
  height: 40px;
  left: 50%;
  animation-duration: 8s;
}

.circle:nth-child(4) {
  width: 50px;
  height: 50px;
  left: 75%;
  animation-duration: 8s;
}

.circle:nth-child(5) {
  width: 60px;
  height: 60px;
  left: 90%;
  animation-duration: 8s;
}

.circle:nth-child(6) {
  width: 70px;
  height: 70px;
  left: 10%;
  animation-duration: 8s;
}

.circle:nth-child(7) {
  width: 80px;
  height: 80px;
  left: 40%;
  animation-duration: 8s;
}

.circle:nth-child(8) {
  width: 90px;
  height: 90px;
  left: 65%;
  animation-duration: 8s;
}

.circle:nth-child(9) {
  width: 100px;
  height: 100px;
  left: 80%;
  animation-duration: 8s;
}

.circle:nth-child(10) {
  width: 110px;
  height: 110px;
  left: 55%;
  animation-duration: 8s;
}

@keyframes float {
  0% {
    transform: translateY(100%);
  }
  50% {
    transform: translateY(-100%);
  }
  100% {
    transform: translateY(100%);
  }
}

.logo {
  position: absolute;
  top: 20px;
  left: 20px;
  max-width: 15%;
  height: auto;
}
</style>
