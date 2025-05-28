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
import axios from 'axios';


    export default {
    setup() {
        const email = ref('');
        const password = ref('');
        const errorMessage = ref('');
        const router = useRouter();

        // ฟังก์ชันสำหรับจัดการการเข้าสู่ระบบ
        const handleSubmit = async () => {
            try {
                const response = await axios.post('https://localhost:7263/api/Auth/login', {
                    UserName: email.value,
                    PassWord: password.value
                });

                console.log("Received data:", response.data);
                localStorage.setItem('token', response.data.token);
                router.push('/UserPage');
            } catch (error) {
                if (error.response && error.response.status === 401) {
                    errorMessage.value = 'ชื่อผู้ใช้หรือรหัสผ่านไม่ถูกต้อง';
                } else {
                    errorMessage.value = 'เกิดข้อผิดพลาดในการเข้าสู่ระบบ';
                    console.error(error);
                }
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
