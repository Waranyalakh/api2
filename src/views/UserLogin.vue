<template>
  <div>
    <h1>Web API</h1>
    <!-- ฟอร์มสำหรับการเข้าสู่ระบบ -->
    <form @submit.prevent="handleSubmit" class="login-form">
      <div>
        <label for="username">Username: </label>
        <input type="text" v-model="username" required />
      </div>
      <br />
      <div>
        <label for="password">Password: </label>
        <input type="password" v-model="password" required />
      </div>
      <button type="submit">Login</button>
    </form>
    <p v-if="errorMessage">{{ errorMessage }}</p>
  </div>
</template>

<script>

import "../assets/style.css";
import { useRouter } from "vue-router";
import { ref } from "vue";
import axios from "axios";
// import api from "../services/api";
export default {
  setup() {
    const username = ref("");
    const password = ref("");
    const errorMessage = ref("");
    const router = useRouter();
  
    // ฟังก์ชันสำหรับจัดการการเข้าสู่ระบบ
    const handleSubmit = async () => {
      try {
        // ส่งคำขอ POST ไปยัง backend เพื่อทำการเข้าสู่ระบบ
        const response = await axios.post(
          "https://localhost:7263/api/Auth/login",
          {
            userName: username.value, // ใช้ email เป็น UserName
            passWord: password.value, // ใช้ password ที่กรอกในฟอร์ม
          },
          {
            headers: {
              "Content-Type": "application/json", // กำหนดประเภทของเนื้อหาที่ส่งเป็น JSON
            },
          }
        );

        // ตรวจสอบสถานะการตอบกลับ
        if (response.data && response.data.token) {
          // แสดงข้อมูลทั้งหมดที่ได้รับใน console
          console.log("Received data:", response.data);
          // เก็บ JWT token ใน localStorage
          localStorage.setItem("token", response.data.token);
          // เปลี่ยนเส้นทางไปที่หน้าผู้ใช้
          router.push("/UserPage");
        } else {
          // แสดงข้อความข้อผิดพลาดหากชื่อผู้ใช้หรือรหัสผ่านไม่ถูกต้อง
          errorMessage.value = "ชื่อผู้ใช้หรือรหัสผ่านไม่ถูกต้อง";
        }
      } catch (error) {
        // แสดงข้อความข้อผิดพลาดเมื่อมีปัญหาในการเชื่อมต่อกับ backend
        errorMessage.value = "เกิดข้อผิดพลาดในการเข้าสู่ระบบ";
      }
    };

    return {
      username,
      password,
      errorMessage,
      handleSubmit,
    };
  },
};

</script>

<style>
/* ใส่สไตล์ CSS ของคุณที่นี่ */
</style>
