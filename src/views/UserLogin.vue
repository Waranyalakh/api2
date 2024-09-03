<template>
    <div>
        <h1>Web API</h1> 
        <!-- ฟอร์มสำหรับการเข้าสู่ระบบ -->
        <form @submit.prevent="handleSubmit" class="login-form">
            <div>
                <label for="email">Username: </label>
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
import '../assets/style.css'; // นำเข้าไฟล์สไตล์จากโฟลเดอร์ assets
import { useRouter } from 'vue-router';
import { ref, onMounted } from ' vue';

export default {
    setup() {
        const email = ref('');
        const password = ref('');
        const errorMessage = ref('');
        const router = useRouter();

        // ตรวจสอบ token เมื่อคอมโพเนนต์ถูก mount
        onMounted(async () => {
            const token = localStorage.getItem('token');
            
            if (token) {
                try {
                    const response = await fetch('http://localhost:5246/api/Auth/login', {
                        method: 'GET',
                        headers: {
                            'Authorization': `Bearer ${token}`
                        }
                    });

                    if (response.ok) {
                        console.log("Access to protected endpoint successful");
                        // สามารถจัดการกับข้อมูลที่ได้จาก response ได้ที่นี่
                    } else {
                        console.error("Access denied");
                        // อาจต้องจัดการ token ที่ไม่ถูกต้องหรือหมดอายุ
                    }
                } catch (error) {
                    console.error("Error while accessing protected endpoint:", error);
                }
            }
        });

        const handleSubmit = async () => {
            try {
                // ส่งคำขอ POST ไปยัง backend เพื่อทำการเข้าสู่ระบบ
                const response = await fetch('http://localhost:5246/api/Auth/login', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        UserName: email.value,  // ใช้ email เป็น UserName
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
                    errorMessage.value = 'ชื่อผู้ใช้หรือรหัสผ่านไม่ถูกต้อง';
                }
            } catch (error) {
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
