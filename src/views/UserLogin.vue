<template>
    <div>
        <h1>Web API</h1>
        <!-- ฟอร์มสำหรับการเข้าสู่ระบบ -->
        <form @submit.prevent="handleSubmit" class="login-form">
            <div>
                <label for="username">Username: </label>
                <input type="text" v-model="email" required>
            </div><br>
            <div>
                <label for="password">Password: </label>
                <input type="password" v-model="password" required>
            </div>
            <button type="submit">Login</button>
        </form>
        <p v-if="errorMessage">{{ errorMessage }}</p>
    </div>
</template>

<script>
import '../assets/style.css';
import { useRouter } from 'vue-router';
import { ref } from 'vue';

export default {
    setup() {
        const email = ref('');
        const password = ref('');
        const errorMessage = ref('');
        const router = useRouter();

        // ฟังก์ชันสำหรับจัดการการเข้าสู่ระบบ
        const handleSubmit = async () => {
            try {
                // ส่งคำขอ POST ไปยัง backend เพื่อทำการเข้าสู่ระบบ
                const response = await fetch('http://localhost:5246/api/Auth/login', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        UserName: email.value, // ใช้ email เป็น UserName
                        PassWord: password.value // ใช้ password ที่กรอกในฟอร์ม
                    })
                });

                // ตรวจสอบสถานะการตอบกลับ
                if (response.ok) {
                    const data = await response.json();
                    // แสดงข้อมูลทั้งหมดที่ได้รับใน console
                    console.log("Received data:", data);
                    // เก็บ JWT token ใน localStorage
                    localStorage.setItem('token', data.token);
                    // เปลี่ยนเส้นทางไปที่หน้าผู้ใช้
                    router.push('/UserPage');
                } else {
                    // แสดงข้อความข้อผิดพลาดหากชื่อผู้ใช้หรือรหัสผ่านไม่ถูกต้อง
                    errorMessage.value = 'ชื่อผู้ใช้หรือรหัสผ่านไม่ถูกต้อง';
                }
            } catch (error) {
                // แสดงข้อความข้อผิดพลาดเมื่อมีปัญหาในการเชื่อมต่อกับ backend
                errorMessage.value = 'เกิดข้อผิดพลาดในการเข้าสู่ระบบ';
            }
        };

        return {
            email,
            password,
            errorMessage,
            handleSubmit
        };
    }
}
</script>

<style>
/* ใส่สไตล์ CSS ของคุณที่นี่ */
</style>
