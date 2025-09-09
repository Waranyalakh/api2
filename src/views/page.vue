
<!-- homepage fpr dispaly button user and item for go that page -->
<template>
  <div>
    <h1>Web API</h1>
    <button @click="goToUserPage">User</button>
    <!-- เพิ่ม @click สำหรับการจัดการการคลิก -->
    <button @click="gotoGradeCalculatorPage">Cal Grade</button>
    <button @click="goToItemPage">item</button>
  </div>
</template>

<script>
import "../assets/style.css";
import { useRouter } from "vue-router"; // นำเข้า useRouter จาก vue-router

export default {
  name: "HomePage",
  setup() {
    const router = useRouter(); // สร้างอินสแตนซ์ของ useRouter

    const goToUserPage = () => {
      router.push("/User"); // เปลี่ยนเส้นทางไปยัง /UserPage เมื่อกดปุ่ม
    };

    const gotoGradeCalculatorPage = () => {
      router.push("/GradeCalculatorPage"); //เปลี่ยนเส้นทางไปที่ /GradeCalculatorPage เมื่อมีการกดปุ่ม
    };

    const goToItemPage = () => {
      router.push("/ItemPage"); // เปลี่ยนเส้นทางไปยัง /ItemPage เมื่อกดปุ่ม
    };

    return {
      goToUserPage,
      gotoGradeCalculatorPage,
      goToItemPage,
    };
  },
};
</script>

<style>
/* ใส่สไตล์ CSS ของคุณที่นี่ */
</style>