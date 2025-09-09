<template>
  <h1>Item Page</h1>
  <!-- show all item  -->
  <div>
    <!-- ถ้า ไอเทมไม่มีหรือเข้าผิดจะขึ้น โหลด -->
    <p v-if="items.length === 0">Loading item..</p>
    <ul v-else>
      <li v-for="(item, index) in items" :key="index">
        <span>Item ID: {{ item.Id }}</span>
        <span>Item Name: {{ item.name }}</span>
        <!-- add button -->
        <button class="btn-Edit" @click="startEdit(item)">Edit</button>
        <button class="btn-Delete" @click="deleteitem(item.id)">Delete</button>
      </li>
    </ul>
  </div>
  <!-- edit item -->
   <div v-if="editItem">
    <h1>Edit item</h1>
    <input type="text" v-model="editItem.name" placeholder="Edit item name" />
    <button @click="updateitem">Save</button>
    <button @click="canceledit">Cancel</button>
   </div>
  <!-- back to previos page -->
  <div>
    <button @click="gotohomepage">Back</button>
  </div>
</template>

<script>
import router from "@/router";
import axios from "axios";




export default {
  name: "ItemPage",
  data() {
    return {
      items: [],
      editItem: null,
    };
  },
  methods: {

    // back to homepage
    gotohomepage() {
      router.push("/UserPage");
    },
    // get all item
    fetchItem() {
      axios
        .get("https://localhost:7263/api/Item", {
          headers: { Authorization: "Bearer" + localStorage.getItem("token") },
        })
        .then((response) => {
          this.items = response.data;
        })
        .catch((error) => {
          console.error("Error fetching data:", error);
        });
    },
    // eidt item
    startEdit(items){
      this.editItem = { ...items };
    },
    canceledit(){
      this.editItem = null;
    },
    // update data
    updateitem(){
      axios.put(`https://localhost:7263/api/Item/${this.editItem.id}`, this.editItem,{
        headers:{'Authorization': 'Bearer'+localStorage.getItem('token')}
      })
      .then((response)=>{
        console.log("Item updated:",response.data);
        this.fetchItem();
        this.canceledit();
      })
      .catch((error)=>{
        console.error("Error updating item:",error);
      });
    },
    deleteitem(id){
      if(!confirm("Do you want to delete this item?")){
        axios.delete(`https://localhost:7263/api/User/${id}`,{
          headers:{'Authorization': 'Bearer'+localStorage.getItem('token')}
        })
        .then((response)=>{
          console.log("item deleted:",response.data);
          this.fetchItem();
        })
        .catch((error)=>{
          console.error("Error deleting item:",error);
        });
      }
    
    }


  },
  mounted() {
    this.fetchItem();
  },
};
</script>

<style>
</style>