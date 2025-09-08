<template>
  <div>
    <h1>Calculate Grade</h1>

    <!-- คำนวณเกรดจากคะแนน -->
    <div class="Num-socre-container">
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
      <div v-if="gradeResult && gradeResult.grade">
        <h2>Grade: {{ gradeResult.grade }}</h2>
      </div>
      <div v-if="gradeError" style="color: red">
        <p>{{ gradeError }}</p>
      </div>
    </div>

    <!-- แสดงช่วงคะแนนสำหรับเกรด -->
    <div class="text-score-container">
      <div>
        <label for="grade">Enter Grade:</label>
        <input
          v-model="grade"
          id="grade"
          type="text"
          placeholder="Enter grade"
        />
        <button @click="fetchScoreRange">Get Score Range</button>
      </div>
      <div v-if="rangeResult">
        <h2>
          Score Range for Grade
          {{ rangeResult.Calculate?.Grade || rangeResult.grade }}
        </h2>
        <p>{{ rangeResult.Message || rangeResult.message }}</p>
      </div>

      <div v-if="rangeError" style="color: red">
        <p>{{ rangeError }}</p>
      </div>
    </div>
  </div>
  <div>
    <div>
      <button @click="gotohomepage">Back</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";

const API_URL1 = "https://localhost:7263/api/CalculateGrade/Calculate";
const API_URL2 = "https://localhost:7263/api/CalculateGrade/ScoreRange";

const router = useRouter();

const score = ref(null);
const grade = ref("");
const gradeResult = ref(null);
const rangeResult = ref(null);
const gradeError = ref(null);
const rangeError = ref(null);

// methods
const gotohomepage = () => {
  router.push("/UserPage");
};

const fetchGrade = async () => {
  gradeError.value = null;
  try {
    const response = await axios.get(API_URL1, {
      params: { score: score.value },
    });
    console.log("Grade API Response:", response.data);
    gradeResult.value = response.data;
  } catch (error) {
    console.log("Error fetching grade:", error);
    gradeError.value = "Error fetching grade. Please check your input.";
  }
};

const fetchScoreRange = async () => {
  rangeError.value = null;
  try {
    const response = await axios.get(API_URL2, {
      params: { grade: grade.value },
    });
    console.log("Score Range API Response:", response.data);
    rangeResult.value = response.data;
  } catch (error) {
    console.error("Error fetching score range:", error);
    rangeError.value = "Error fetching score range. Please check your input.";
  }
};
</script>

<style scoped>
/* เพิ่มสไตล์สำหรับ error */
/* * {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
} */
* {
  margin-top: 20px;
}
p {
  margin: 0;
}

h2,
h3 {
  margin-top: 20px;
}
/* .Num-socre-container {
  background-color: aquamarine;
  padding: 40px;
  border-radius: 10px;
}
.text-score-container {
  background-color: cadetblue;
  padding: 40px;
  margin-top: 30px;
  border-radius: 10px;
} */
.error-message {
  color: red;
}
</style>
