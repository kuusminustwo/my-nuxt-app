<template>
<div class="container">
    <img src="../pic/logo2.png" alt="Logo" class="main_logo">

    <div class="student-card" ref="studentCard"  v-if="student">
    <div class="header">
        <div>
        <table>
            <tr>
            <td style="width: 20%; text-align: center;"><img src="../pic/logo2.png" class="card_logo"></td>
            <td style="width: 60%; text-align: center;">
                <h2 style="font-size: 30px;">Шинэ Монгол Эрдмийн Хүрээлэн</h2>
                <h3 style="font-size: 24px;">{{ school.name }}</h3>
                <span style="font-size: 11px;">{{ school.address }}</span><br>
                <span style="font-size: 11px;">Утас/факс: {{ school.phone }}</span><br>
                <span style="font-size: 11px;">Цахим шуудан: {{ school.email }}</span>
            </td>
            <td style="width: 20%; text-align: center;"><img :src="profile" class="profile"></td>
            </tr>
        </table>
        </div>
    </div>

    <div class="student-info">
        <p>Суралцагчийн тодорхойлолт</p>
        <p class="text">{{ student.firstname }} овогтой {{ student.lastname }} нь тус сургуулийн {{ student.major }} хөтөлбөрөөр сурдаг нь үнэн болно.</p>
    </div>

    <div class="footer">
        <img :src="qr" alt="QR" class="qr">
    </div>
    </div>

    <div class="buttons">
    <button @click="printCard">Хэвлэх</button>
    <button @click="goBack">Буцах</button>
    </div>
</div>
</template>

<script>
import { ref } from 'vue'

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
      const storedToken = localStorage.getItem('token')
      if (!storedToken) {
        throw new Error('No token available')
      }

      const response = await this.$axios.$get('/tod', {
        headers: {
          Authorization: `Bearer ${storedToken}`
        }
      })
      console.log(response)
      this.user = response.data.user
      this.student = response.data.student
      this.token = storedToken
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
    goBack () {
      this.$router.push('/profile')
    },
    printCard () {
      const printContents = this.$refs.studentCard.outerHTML
      const originalContents = document.body.innerHTML

      document.body.innerHTML = printContents
      window.print()
      document.body.innerHTML = originalContents
      location.reload()
    }
  }
}
</script>

<script setup>
// Additional constants
const school = {
  name: 'Шинэ Монгол Технологийн Дээд Сургууль',
  address: 'Улаанбаатар хот, Баянзүрх дүүрэг, 25-р хороо, 13-р хороолол, Манлайбаатар Дамдинсүрэнгийн гудамж-43/1',
  phone: '75777799',
  email: 'info@nmit.edu.mn'
}

const profile = 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQIzUtb-iA6I8gnWwLY1_xrZN7koyQNqmNQCw&s'
const qr = 'https://upload.wikimedia.org/wikipedia/commons/d/d0/QR_code_for_mobile_English_Wikipedia.svg'

const studentCard = ref(null)
</script>

  <style scoped>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    height: 100vh;
    text-align: center;
    font-family: Roboto, sans-serif;
  }

  .main_logo {
    position: absolute;
    top: 20px;
    left: 20px;
    max-width: 15%;
    height: auto;
  }

  .student-card {
    position: relative;
    border: 1px solid #ddd;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    width: 210mm;
    height: 297mm;
    background: white;
  }

  .header {
    width: 100%;
    margin-bottom: 20px;
  }

  .profile {
    height: 100px;
  }

  .card_logo {
    max-width: 180px;
    height: auto;
    top: 20px;
    left: 20px;
  }

  .footer {
    position: absolute;
    bottom: 20px;
    left: 20px;
  }

  .qr {
    max-width: 100px;
    height: auto;
  }

  .buttons {
    margin-top: 20px;
    position: relative;
    bottom: 30px;
  }

  button {
    padding: 10px 20px;
    font-size: 16px;
    background-color: #48C9B0;
    color: white;
    border: none;
    border-radius: 5px;
    transition: background-color 0.3s;
    width: 150px;
    margin: 10px
  }

  /* Printing Styles */
  @media print {
    .main_logo, .buttons {
      display: none;
    }

    .student-card {
      width: 100%;
      height: auto;
      border: none;
      box-shadow: none;
      margin: 0;
      padding: 0;
    }
  }
  </style>
