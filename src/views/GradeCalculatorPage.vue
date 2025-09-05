<template>
  <div>
    <h1>Calculate Grade</h1>

    <!-- คำนวณเกรดจากคะแนน -->
    <div>
      <label for="score">Enter Score:</label>
      <input
        v-model.number="score"
        id="score"
        type="number"
        placeholder="Enter score"
      />
      <button @click="fetchGrade">Get Grade</button>
    </div>
    <div v-if="gradeResult && gradeResult.Grade">
      <h2>Grade: {{ gradeResult.Grade }}</h2> 
      <!-- h2 not show -->
    </div>
    <div v-if="gradeError" style="color: red">
      <p>{{ gradeError }}</p>
      <!-- p not show -->
    </div>

    <!-- แสดงช่วงคะแนนสำหรับเกรด -->
    <div>
      <label for="grade">Enter Grade:</label>
      <input v-model="grade" id="grade" type="text" placeholder="Enter grade" />
      <button @click="fetchScoreRange">Get Score Range</button>
    </div>
    <div
      v-if="rangeResult && rangeResult.Calculate && rangeResult.Calculate.Grade"
    >
      <h3>Score Range for Grade {{ rangeResult.Calculate.Grade }}:</h3>
      <p>{{ rangeResult.Message }}</p>
    </div>

    <div v-if="rangeError" style="color: red">
      <p>{{ rangeError }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

const API_URL1 = "https://localhost:7263/api/CalculateGrade/Calculate";
const API_URL2 = "https://localhost:7263/api/CalculateGrade/ScoreRange";

export default {
  name: "CalculateGradePage",
  data() {
    return {
      score: null,
      grade: "",
      gradeResult: null,
      rangeResult: null,
      gradeError: null, // เพิ่มตัวแปรจัดการ error ของเกรด
      rangeError: null, // เพิ่มตัวแปรจัดการ error ของช่วงคะแนน
    };
  },
  methods: {
    async fetchGrade() {
      this.gradeError = null; // ล้างค่า error ก่อน
      try {
        const response = await axios.get(API_URL1, {
          params: { score: this.score },
        });
        console.log("Grade API Response:", response.data); // ตรวจสอบผลลัพธ์จาก API
        this.gradeResult = response.data;
      } catch (error) {
        console.log("Error fetching grade:", error);
        this.gradeError = "Error fetching grade. Please check your input.";
      }
    },
    async fetchScoreRange() {
      this.rangeError = null; // ล้างค่า error ก่อน
      try {
        const response = await axios.get(API_URL2, {
          params: { grade: this.grade },
        });
        console.log("Score Range API Response:", response.data); // ตรวจสอบผลลัพธ์จาก API
        this.rangeResult = response.data;
      } catch (error) {
        console.error("Error fetching score range:", error);
        this.rangeError =
          "Error fetching score range. Please check your input.";
      }
    },
  },
};
</script>

<style scoped>
/* เพิ่มสไตล์สำหรับ error */
p {
  margin: 0;
}

h2,
h3 {
  margin-top: 20px;
}
</style>
