<template>
  <div class="container">
    <img src="../pic/logo2.png" alt="Logo" class="main_logo">

    <div class="student-card" v-if="student">
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
        <p>Оюутны мэдээлэл</p>
        <div class="info-frame">
          <div class="info-row">
            <div class="info-label">Эцэг/эхийн нэр</div>
            <div class="info-value">{{ student.firstname }}</div>
            <div class="info-label">Өөрийн нэр</div>
            <div class="info-value">{{ student.lastname }}</div>
          </div>
          <div class="info-row">
            <div class="info-label">Регистр</div>
            <div class="info-value">{{ student.register }}</div>
            <div class="info-label">Оюутны код</div>
            <div class="info-value">{{ student.student_id }}</div>
          </div>
        </div>

        <p>Хичээлийн жил</p>
        <table class="grades-table">
          <thead>
            <tr>
              <th>Он/сар</th>
              <th>Хичээл</th>
              <th>Кредит</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(course, index) in courses" :key="index">
              <td>{{ course.year }}</td>
              <td>{{ course.subject }}</td>
              <td>{{ course.credit }}</td>
            </tr>
            <tr>
              <td colspan="2" style="text-align: center;"><strong>Нийт</strong></td>
              <td><strong>{{ totalCredits }}</strong></td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="footer">
        <!-- <img :src="qr" alt="QR" class="qr"> -->
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

      const response = await this.$axios.$get('/hicheel', {
        headers: {
          Authorization: `Bearer ${storedToken}`
        }
      })

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
// Sample student data

const school = {
  name: 'Шинэ Монгол Технологийн Дээд Сургууль',
  address: 'Улаанбаатар хот, Баянзүрх дүүрэг, 25-р хороо, 13-р хороолол, Манлайбаатар Дамдинсүрэнгийн гудамж-43/1',
  phone: '75777799',
  email: 'info@nmit.edu.mn'
}

const profile = 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQIzUtb-iA6I8gnWwLY1_xrZN7koyQNqmNQCw&s'
// const qr = 'https://upload.wikimedia.org/wikipedia/commons/d/d0/QR_code_for_mobile_English_Wikipedia.svg'

// Sample list of courses
const courses = ref([
  { year: '2023-2024', subject: 'Математик', credit: '3' },
  { year: '2023-2024', subject: 'Магадлал ба статистик', credit: '3' },
  { year: '2023-2024', subject: 'Хиймэл оюун ухааны үндэс ба машин сургалт', credit: '3' },
  { year: '2023-2024', subject: 'Цахим аюулгүй байдал', credit: '3' },
  { year: '2023-2024', subject: 'Компьютерийн сүлжээ', credit: '3' },
  { year: '2023-2024', subject: 'Мобайл програмчлал', credit: '3' }
])

// Calculate total credits
const totalCredits = courses.value.reduce((acc, course) => acc + parseFloat(course.credit), 0)
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
  padding: 20px;
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

.student-info {
  margin: 20px;
}

.info-frame {
  border: 1px solid #ddd;
  padding: 10px;
  margin-bottom: 20px;
}

.info-row {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
}

.info-label {
  font-weight: bold;
  width: 25%;
}

.grades-table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 20px;
}

.grades-table th, .grades-table td {
  border: 1px solid #ddd;
  padding: 8px;
}

.grades-table th {
  background-color: #f2f2f2;
  text-align: center;
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
  margin: 10px;
}

button:hover {
  background-color: #3b9b8c;
}
</style>
