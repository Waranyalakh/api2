<template>
    <nav>
       
        <router-link to="/login">Login</router-link>
    </nav>

    <!--<button @click="goToLogin">Go to Login</button>
    <button @click="gotohome">Go to Home</button>-->

    <router-view />
</template>

<script>
    import './assets/style.css'
    import { useRouter } from 'vue-router'

    export default {
        setup() {
            const router = useRouter()

            const goToLogin = () => {
                router.push('/login')
            }

            const gotohome = () => {
                router.push('/UserPage')
            }

            return {
                goToLogin,
                gotohome
            }
        }
    }
</script>

<style>
    /* ใส่สไตล์ตามต้องการ */
</style>
