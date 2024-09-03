<template>
  <nav>
    <!-- <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>| -->
     <router-link to="/login">login</router-link> 
    
  </nav>
  <router-view/>

</template>
<script>
import './assets/style.css' //import style form assets folder

import { useRouter } from 'vue-router'; //import router
export default {
 components:{

 },
  setup() {
    const router = useRouter();
    const goToLogin = () => {
      router.push('/login'); // เปลี่ยนเส้นทางไปยัง /login
    };
    const gotohome = () =>{
      router.push('/UserPage')
    }
    return{
      goToLogin,
      gotohome
    }
  },
}
</script>
<style>

</style>
