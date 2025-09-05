<template>
  <nav>
    <!-- <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>| -->
    <!-- <router-link to="/login">login</router-link> -->
  </nav>
  <router-view />
</template>
<script>
import "./assets/style.css"; //import style form assets folder

import { useRouter } from "vue-router"; //import router
import { onMounted } from "vue"; //import ref from vue
export default {
  components: {},
  setup() {
    onMounted(() => {
      // ใช้ router.push() เพื่อไปยังหน้า /login หลังจากที่คอมโพเนนต์โหลดเสร็จ
      router.push("/login");
    });
    const router = useRouter();
    const goToLogin = () => {
      router.push("/login"); // เปลี่ยนเส้นทางไปยัง /login
    };
    const gotohome = () => {
      router.push("/UserPage");
    };
    return {
      goToLogin,
      gotohome,
    };
  },
};
</script>
<style>
</style>
