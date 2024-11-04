<template>
    <div class="main container">
      <div class="user-management">
        <!-- Form and Title Section -->
        <div class="form-container" ref="formContainer"> <!-- Add ref here -->
          <h1>User Management</h1>
          <form @submit.prevent="isEditing ? updateUser() : addUser()" class="mb-4">
            <div class="form-group">
              <input
                v-model="currentUser.name"
                type="text"
                class="form-control input-lg"
                placeholder="Enter Name"
                required
              />
            </div>
            <div class="form-group">
              <input
                v-model="currentUser.email"
                type="email"
                class="form-control input-lg"
                placeholder="Enter Email"
                required
              />
            </div>
            <div class="form-group">
              <select
                v-model="currentUser.gender"
                class="form-control input-lg"
                required
              >
                <option disabled value="">Select Gender</option>
                <option>Male</option>
                <option>Female</option>
                <option>Prefer not to say</option>
              </select>
            </div>
            <div class="form-group">
              <input
                v-model="currentUser.age"
                type="number"
                class="form-control input-lg"
                placeholder="Enter Age"
                required
              />
            </div>
            <div class="form-group">
              <input
                v-model="currentUser.department"
                type="text"
                class="form-control input-lg"
                placeholder="Enter Department"
                required
              />
            </div>
            <button type="submit" class="btn btn-primary">{{ isEditing ? 'Update User' : 'Add User' }}</button>
            <button v-if="isEditing" @click="cancelEdit" type="button" class="btn btn-secondary">Cancel</button>
          </form>
          
          <!-- Alert message -->
          <div v-if="showAlert" class="alert alert-success">
            User Information Successfully Updated
          </div>
        </div>
  
        <!-- User Cards Container -->
        <div class="user-cards-container">
          <div class="user-list">
            <div v-for="user in users" :key="user.id" class="user-card">
              <div class="card h-100">
                <div class="card-body d-flex flex-column">
                  <h3 class="card-title">{{ user.name }}</h3>
                  <p class="card-text">{{ user.email }}</p>
                  <p class="card-text">Gender: {{ user.gender }}</p>
                  <p class="card-text">Age: {{ user.age }}</p>
                  <p class="card-text">Department: {{ user.department }}</p>
                  <div class="mt-auto">
                    <button @click="editUser(user)" class="btn btn-warning">Edit</button>
                    <button @click="deleteUser(user.id)" class="btn btn-danger">Delete</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue';
  
  // Reactive state
  const users = ref([]);
  const newUser = ref({ name: '', email: '', gender: '', age: null, department: '' });
  const editUserData = ref({ id: null, name: '', email: '', gender: '', age: null, department: '' });
  const isEditing = ref(false);
  const showAlert = ref(false); // State for alert visibility
  const formContainer = ref(null); // Reference for the form container
  
  // Computed property to manage the current user object
  const currentUser = computed(() => (isEditing.value ? editUserData.value : newUser.value));
  
  // Function to add a new user
  const addUser = () => {
    const userId = Date.now(); // Create a unique ID based on the current timestamp
    users.value.push({ id: userId, ...newUser.value }); // Push new user data to the users array
    resetForm(); // Reset form after adding a user
  };
  
  // Function to initiate editing a user
  const editUser = (user) => {
    editUserData.value = { ...user }; // Populate editUserData with the user's information
    isEditing.value = true; // Set editing state to true
    // Scroll to the form container
    formContainer.value.scrollIntoView({ behavior: 'smooth', block: 'start' });
  };
  
  // Function to update user data
  const updateUser = () => {
    const index = users.value.findIndex((user) => user.id === editUserData.value.id); // Find user by ID
    if (index !== -1) {
      users.value[index] = { ...editUserData.value }; // Update the user with new data
      showAlert.value = true; // Show alert after updating
      setTimeout(() => showAlert.value = false, 3000); // Hide alert after 3 seconds
      cancelEdit(); // Reset editing state after updating
    }
  };
  
  // Function to delete a user
  const deleteUser = (id) => {
    users.value = users.value.filter(user => user.id !== id); // Remove user from the array
  };
  
  // Function to cancel editing
  const cancelEdit = () => {
    isEditing.value = false; // Reset editing state
    resetForm(); // Clear the form fields
  };
  
  // Function to reset the form fields
  const resetForm = () => {
    newUser.value = { name: '', email: '', gender: '', age: null, department: '' }; // Reset new user fields
    editUserData.value = { id: null, name: '', email: '', gender: '', age: null, department: '' }; // Reset edit user fields
  };
  </script>
  
  <style scoped>
  .user-management {
    display: flex;
    flex-direction: column;
    gap: 20px;
    margin-top: 20px;
  }
  
  .form-container {
    max-width: 400px; /* Limit form width */
    margin-left: 20px; /* Push form section to the left */
  }
  
  .form-control {
    margin-bottom: 15px;
  }
  
  .user-cards-container {
    max-width: 800px; /* Set max width for user cards container */
    width: 100%;
    display: flex;
    justify-content: center; /* Center user cards container */
    margin: 20px auto; /* Center on page */
  }
  
  .user-list {
    display: flex;
    flex-direction: column;
    gap: 20px;
    width: 100%;
  }
  
  .user-card {
    width: 100%;
  }
  
  .card {
    border: 1px solid #ccc;
    width: 100%;
  }
  
  .btn-danger {
    margin-left: 1rem;
  }
  .btn-primary{
    margin-right: 1rem;
  }
  
  .main {
    background-color: rgb(160, 160, 160);
    border: 2px solid black;
    border-radius: 2%;
    margin-top: 1rem;
  }
  
  /* Styles for alert message */
  .alert {
    margin-top: 15px;
    padding: 10px;
    background-color: #d4edda;
    color: #155724;
    border: 1px solid #c3e6cb;
    border-radius: 5px;
  }
  </style>
  