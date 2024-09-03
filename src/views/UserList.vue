<template>
  <div>
    <h1>User List</h1> 

    <!-- Input form สำหรับเพิ่มผู้ใช้ใหม่ -->
    <div>
      <input v-model="newUser.name" placeholder="Enter name" />
      <button @click="addUser">Submit</button>
    </div>
    
    <ul>
      <li v-for="user in users" :key="user.id" class="user-item">
        <span class="user-id">ID: {{ user.id }}</span>
        <span class="user-name">Name: {{ user.name }}</span>

        <!-- Edit and Delete Buttons -->
        <button @click="editUser(user)">Edit</button>
        <button class="btn-Delete" @click="deleteUser(user.id)">Delete</button>
      </li>
    </ul>

    <!-- Edit User Form -->
    <div v-if="editingUser">
      <h2>Edit User</h2>
      <input v-model="editingUser.name" placeholder="Edit name" />
      <button @click="updateUser">Update</button>
      <button @click="cancelEdit">Cancel</button>
    </div>

    <!-- Fetch User by ID Form -->
    <div>
      <h1>Get by Id</h1>
      <input v-model="fetchUserId" placeholder="Enter user ID to fetch" />
      <button @click="fetchUserById">Fetch User</button>
    </div>
    
    <!-- Display fetched user details -->
    <div v-if="fetchedUser">
      <h2>Fetched User Details</h2>
      <p>ID: {{ fetchedUser.id }}</p>
      <p>Name: {{ fetchedUser.name }}</p>
    </div>
  </div>
</template>

<script>
import '../assets/style.css' //import style from assets folder

import axios from "axios"; //import library axios

export default {
  name: 'UserPage',
  data() {
    return {
      users: [],
      newUser: {
        name: "",
      },
      editingUser: null, // To track the user being edited
      fetchUserId: "", // ID to fetch user by
      fetchedUser: null, // Fetched user details
    };
  },
  mounted() {
    this.fetchUsers();
  },
  methods: {
    fetchUsers() {
      axios
        .get("http://localhost:5246/api/User",{headers:{ 'Authorization': 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ0ZXN0dXNlciIsImp0aSI6ImRiODk2ZDFmLWQzNzUtNDQyMi05NTc2LTdiMDlmYjU3NmI1YyIsImV4cCI6MTcyNTM0MjA5NSwiaXNzIjoiWW91cklzc3VlciIsImF1ZCI6IllvdXJBdWRpZW5jZSJ9.FBNJNq9120u3v_hjJlmYcioa9AD42RLs-J90doh7bJc'}})
        .then((response) => {
          this.users = response.data;
        })
        .catch((error) => {
          console.error("Error fetching data:", error);
        });
    },
    addUser() {
      if (this.newUser.name.trim() === "") {
        alert("กรุณากรอกชื่อผู้ใช้");
        return;
      }

      axios
        .post("http://localhost:5246/api/User", this.newUser)
        .then((response) => {
          console.log("User added successfully:", response.data);
          this.users.push(response.data);
          this.newUser.name = "";
        })
        .catch((error) => {
          console.error("Error adding user:", error);
        });
    },
    editUser(user) {
      this.editingUser = { ...user }; // Copy the user data to editingUser
    },
    updateUser() {
      if (!this.editingUser.name.trim()) {
        alert("กรุณากรอกชื่อผู้ใช้");
        return;
      }

      axios
        .put(
          `http://localhost:5246/api/User/${this.editingUser.id}`,
          this.editingUser
        )
        .then((response) => {
          console.log("User updated successfully:", response.data);
          const index = this.users.findIndex(
            (u) => u.id === this.editingUser.id
          );
          if (index !== -1) {
            this.users[index] = response.data; // Update the user in the list
          }
          this.editingUser = null; // Clear the edit form
        })
        .catch((error) => {
          console.error(
            "Error updating user:",
            error.response ? error.response.data : error.message
          );
        });
    },
    deleteUser(id) {
      if (confirm("คุณแน่ใจหรือว่าต้องการลบผู้ใช้นี้?")) {
        axios
          .delete(`http://localhost:5246/api/User/${id}`)
          .then(() => {
            console.log("User deleted successfully");
            this.users = this.users.filter((user) => user.id !== id); // Remove the user from the list
          })
          .catch((error) => {
            console.error(
              "Error deleting user:",
              error.response ? error.response.data : error.message
            );
          });
      }
    },
    cancelEdit() {
      this.editingUser = null; // Cancel the edit
    },
    fetchUserById() {
      if (!this.fetchUserId.trim()) {
        alert("กรุณากรอก ID ของผู้ใช้");
        return;
      }

      axios
        .get(`http://localhost:5246/api/User/${this.fetchUserId}`)
        .then((response) => {
          this.fetchedUser = response.data; // Store the fetched user details
        })
        .catch((error) => {
          console.error("Error fetching user by ID:", error);
        });
    },
  },
};
</script>

<style>
/* Your existing styles */
</style>
