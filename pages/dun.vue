<template>
  <div class="container">
    <img src="../pic/logo2.png" alt="Logo" class="main_logo">

    <div class="student-card"  v-if="student">
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
        <p class="info-section-title">Оюутны мэдээлэл</p>
        <div class="student-details-grid">
          <div class="student-detail-item"><strong>Овог:</strong> {{ student.firstname }}</div>
          <div class="student-detail-item"><strong>Нэр:</strong> {{ student.lastname }}</div>
          <div class="student-detail-item"><strong>Регистрийн дугаар:</strong> {{ student.register }}</div>
          <div class="student-detail-item"><strong>Оюутны код:</strong> {{ student.student_id }}</div>
          <div class="student-detail-item"><strong>Элссэн он:</strong> {{ student.enrolldate }}</div>
          <div class="student-detail-item"><strong>Сургалтын хэлбэр:</strong> {{ student.undergraduate }}</div>
          <div class="student-detail-item"><strong>Мэргэжил:</strong> {{ student.major }}</div>
          <div class="student-detail-item"><strong>Голч дүн:</strong> {{ student.gpa }}</div>
          <div class="student-detail-item"><strong>Нийт кредит:</strong> {{ student.total_credits }}</div>
        </div>

        <div class="grade-info-section">
          <p class="info-section-title">Дүнгийн мэдээлэл</p>
          <table class="grades-table">
            <thead>
              <tr>
                <th>Семестр</th>
                <th>Хичээл</th>
                <th>Кредит</th>
                <th>Оноо</th>
                <th>Дүн</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(grade, index) in grades" :key="index">
                <td>{{ grade.semester }}</td>
                <td>{{ grade.subject }}</td>
                <td>{{ grade.credit }}</td>
                <td>{{ grade.grade }}</td>
                <td>{{ grade.score }}</td>
              </tr>
            </tbody>
          </table>
        </div>
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
      token: '',
      enrollments: []
    }
  },
  async created () {
    try {
      const storedToken = localStorage.getItem('token')
      if (!storedToken) {
        throw new Error('No token available')
      }

      const response = await this.$axios.$get('/dun', {
        headers: {
          Authorization: `Bearer ${storedToken}`
        }
      })
      // console.log(response.enrollments[0].Student)
      // this.student = response.enrollments[0].Student
      console.log(response)
      this.student = response.student
      this.token = storedToken
      this.enrollments = response.enrollments
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

const school = {
  name: 'Шинэ Монгол Технологийн Дээд Сургууль',
  address: 'Улаанбаатар хот, Баянзүрх дүүрэг, 25-р хороо, 13-р хороолол, Манлайбаатар Дамдинсүрэнгийн гудамж-43/1',
  phone: '75777799',
  email: 'info@nmit.edu.mn'
}

const profile = 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQIzUtb-iA6I8gnWwLY1_xrZN7koyQNqmNQCw&s'
// const qr = 'https://upload.wikimedia.org/wikipedia/commons/d/d0/QR_code_for_mobile_English_Wikipedia.svg'

// Sample grade data
const grades = ref([
  { semester: '1-р семестр', subject: 'Математик', credit: 3, grade: '50', score: 'F' },
  { semester: '1-р семестр', subject: 'Физик', credit: 4, grade: '91', score: 'A' },
  { semester: '2-р семестр', subject: 'Хими', credit: 2, grade: '80', score: 'B' }
])
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

.student-info {
  margin: 20px;
}

.student-details-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
  font-size: 10px;
  text-align: left;
}

.student-detail-item {
  padding: 5px;
}

.info-section-title {
  font-weight: bold;
  font-size: 16px;
  margin-bottom: 5px;
}

.grade-info-section {
  border: 1px solid #ddd;
  padding: 10px;
  margin-top: 10px;
}

.grades-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
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
