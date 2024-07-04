<template>
  <div>
    <div class="round-icons">
      <div class="round-icon"></div>
      <div class="round-icon"></div>
      <div class="round-icon"></div>
    </div>
    <div class="login-container">
      <div class="left-half"></div>
      <div class="separator"></div>
      <div class="right-half">
        <div class="animated-shape"></div>
        <div class="login-card fade-in">
          <h2>Нэвтрэх</h2>
          <form @submit.prevent="handleLogin">
            <div class="form-group">
              <label for="username">Хэрэглэгчийн нэр:</label>
              <input type="text" id="username" v-model="username" required @focus="animateInput" />
            </div>
            <div class="form-group">
              <label for="password">Нууц үг:</label>
              <input type="password" id="password" v-model="password" required @focus="animateInput" />
            </div>
            <button @click="handleButtonClick('home')">Нэвтрэх</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const username = ref('');
const password = ref('');

const handleLogin = () => {
  if (username.value === 'user' && password.value === '1234') {
    router.push('/home');
  } else {
    alert('Нэвтрэх нэр эсвэл нууц үг буруу байна');
  }
};

const handleButtonClick = (buttonType) => {
  switch (buttonType) {
    case 'home':
      router.push('/home');
      break;
    default:
      break;
  }
};

const animateInput = (event) => {
  event.target.classList.add('animate-input');
};
</script>

<style scoped>
.login-container {
  display: flex;
  height: 100vh;
}

.left-half {
  flex: 3.4;
  display: flex;
  justify-content: center;
  align-items: center;
  background-image: url('.././pic/shn.jpg'); /* Replace with your image path */
  background-size: cover;
  background-position: center;
}

.separator {
  width: 2px;
  height: 100%;
  background-color: #ddd;
}

.right-half {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative; /* Added for positioning the animated shape */
}

.animated-shape {
  width: 100px;
  height: 100px;
  background-color: #48C9B0;
  border-radius: 50%;
  position: absolute;
  animation: pulsate 2s infinite;
}

@keyframes pulsate {
  0% {
    transform: scale(1);
    opacity: 1;
  }
  50% {
    transform: scale(1.2);
    opacity: 0.7;
  }
  100% {
    transform: scale(1);
    opacity: 1;
  }
}

.login-card {
  border: 1px solid #ddd;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1), 0 0 30px 10px rgba(72, 201, 176, 0.7);
  width: 400px;
  background-color: #fff;
  opacity: 0;
  animation: fadeIn 1s forwards;
  position: relative; /* Added for proper stacking of elements */
  z-index: 1; /* Ensures the login card is above the animated shape */
}

@keyframes fadeIn {
  to {
    opacity: 1;
  }
}

.login-card h2 {
  margin-bottom: 20px;
  font-size: 24px;
}

.form-group {
  margin-bottom: 15px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
}

.form-group input {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
  transition: border-color 0.3s, box-shadow 0.3s;
}

.form-group input:focus {
  border-color: #48C9B0;
  box-shadow: 0 0 8px rgba(72, 201, 176, 0.5);
}

button {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  cursor: pointer;
  background-color: #48C9B0;
  color: #fff;
  border: none;
  border-radius: 5px;
  transition: background-color 0.3s, transform 0.3s;
}

button:hover {
  background-color: #399e92;
  transform: scale(1.05);
}

/* Animation for input fields */
.animate-input {
  animation: inputAnimation 0.3s ease-out;
}

@keyframes inputAnimation {
  from {
    transform: scale(1.05);
  }
  to {
    transform: scale(1);
  }
}

/* Styles for the round icons */
.round-icons {
  position: fixed;
  top: 20px;
  right: 20px;
  display: flex;
  gap: 10px;
  z-index: 2;
}

.round-icon {
  width: 50px;
  height: 50px;
  background-color: #48C9B0;
  border-radius: 50%;
  animation: bounce 2s infinite;
}

@keyframes bounce {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-20px);
  }
}
</style>
