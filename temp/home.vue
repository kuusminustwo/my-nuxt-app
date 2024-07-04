<template>
  <div class="container">
    <!-- Circle Background -->
    <div class="circle-background">
      <div class="circle" v-for="n in 10" :key="n"></div>
    </div>

    <img src="../pic/logo2.png" alt="Logo" class="logo">

    <!-- Menu -->
    <!-- Menu with improved opening animation -->
<div class="menu">
  <button @click="toggleMenu" class="menu-button">Меню</button>
  <transition name="slide">
    <div v-if="menuOpen" class="menu-content">
      <a @click="handleButtonClick('Тодорхойлолт')">Тодорхойлолт</a>
      <a @click="handleButtonClick('Дүнгийн хуудас')">Дүнгийн хуудас</a>
      <a @click="handleButtonClick('Хичээл сонголт')">Хичээл сонголт</a>
      <a @click="handleLogout">Гарах</a>
    </div>
  </transition>
</div>


    <div class="student-card">
      <h2>{{ student.Fname }} овогтой {{ student.Lname }}</h2>
      <p>Нас: {{ student.age }}</p>
      <p>Хөтөлбөр: {{ student.course }}</p>
    </div>

    <div class="button-group">
      <button @click="handleButtonClick('Тодорхойлолт')">Тодорхойлолт</button>
      <button @click="handleButtonClick('Дүнгийн хуудас')">Дүнгийн хуудас</button>
      <button @click="handleButtonClick('Хичээл сонголт')">Хичээл сонголт</button>
      <button @click="handleLogout" class="logout-button">Гарах</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const menuOpen = ref(false)

// Sample student data
const student = {
  Fname: 'Бат',
  Lname: 'Дорж',
  age: 21,
  course: 'ПРОГРАМ ХАНГАМЖ',
}

// Function to handle button clicks and navigate to respective pages
const handleButtonClick = (buttonType) => {
  menuOpen.value = false // Close the menu when a button is clicked
  switch (buttonType) {
    case 'Тодорхойлолт':
      router.push('/tod') // Navigate to '/tod' page
      break
    case 'Дүнгийн хуудас':
      router.push('/dun') // Navigate to '/dun' page
      break
    case 'Хичээл сонголт':
      router.push('/hicheel') // Navigate to '/hicheel' page
      break
    default:
      break
  }
}

// Function to handle logout
const handleLogout = () => {
  // Clear session data if needed
  // e.g., localStorage.removeItem('authToken')

  // Redirect to login page
  router.push('/')
}

// Function to toggle the menu
const toggleMenu = () => {
  menuOpen.value = !menuOpen.value
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

/* Menu styles */
.menu {
  position: absolute;
  top: 20px;
  right: 20px;
}

.menu-button {
  background-color: #48C9B0;
  color: #fff;
  border: none;
  border-radius: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.menu-content {
  display: flex;
  flex-direction: column;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  position: absolute;
  top: 50px;
  right: 0;
}

.menu-content a {
  padding: 10px;
  cursor: pointer;
  text-decoration: none;
  color: #333;
}

.menu-content a:hover {
  background-color: #f5f5f5;
}

.student-card {
  border: 1px solid #ddd;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  max-width: 300px;
  margin-top: 80px; /* Adjusted for the menu */
}

.student-card h2 {
  margin: 0 0 10px;
  font-size: 24px;
}

.student-card p {
  margin: 5px 0;
  font-size: 16px;
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

/* Specific style for the logout button */
.logout-button {
  background-color: #e74c3c; /* Red color */
}

.logout-button:hover {
  background-color: #c0392b; /* Darker red on hover */
}
</style>
