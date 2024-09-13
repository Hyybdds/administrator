<template>
    <div class="p-6">
      <!-- Title and Add Button -->
      <div class="flex justify-between items-center mb-4">
        <h1 class="text-xl font-bold">Product Categories</h1>
        <button @click="openForm" class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600">
          Add
        </button>
      </div>
  
      <!-- Add/Edit Form -->
      <div v-if="isFormVisible" class="mb-6 p-4 border border-gray-200 rounded-lg">
        <h2 class="text-lg font-semibold mb-4">{{ formMode === 'add' ? 'Add Product Category' : 'Edit Product Category' }}</h2>
        <form @submit.prevent="saveCategory">
          <div class="mb-4">
            <label for="id" class="block text-sm font-medium text-gray-700">Id</label>
            <input v-model="form.id" type="text" id="id" class="mt-1 block w-full border border-gray-300 rounded-lg p-2" :readonly="formMode === 'edit'"/>
          </div>
          <div class="mb-4">
            <label for="description" class="block text-sm font-medium text-gray-700">Description</label>
            <input v-model="form.description" type="text" id="description" class="mt-1 block w-full border border-gray-300 rounded-lg p-2"/>
          </div>
          <div class="mb-4">
            <label for="status" class="block text-sm font-medium text-gray-700">Status</label>
            <select v-model="form.status" id="status" class="mt-1 block w-full border border-gray-300 rounded-lg p-2">
              <option value="active">Active</option>
              <option value="inactive">Inactive</option>
            </select>
          </div>
          <div class="flex justify-end">
            <button type="submit" class="bg-green-500 text-white px-4 py-2 rounded hover:bg-green-600">
              Save
            </button>
          </div>
        </form>
      </div>
  
      <!-- Product Categories Table -->
      <div v-if="categories.length === 0" class="text-center text-gray-500">
        No product categories available.
      </div>
      <table v-else class="min-w-full border border-gray-300 rounded-lg">
        <thead class="bg-gray-100">
          <tr>
            <th class="p-2 border-b">Id</th>
            <th class="p-2 border-b">Description</th>
            <th class="p-2 border-b">Status</th>
            <th class="p-2 border-b">Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="category in categories" :key="category.id">
            <td class="p-2 border-b">{{ category.id }}</td>
            <td class="p-2 border-b">{{ category.description }}</td>
            <td class="p-2 border-b">{{ category.status }}</td>
            <td class="p-2 border-b flex space-x-2">
              <!-- View Icon -->
              <button @click="viewCategory(category)" class="text-blue-500 hover:text-blue-700">
                <i class="fas fa-eye"></i>
              </button>
              <!-- Edit Icon -->
              <button @click="editCategory(category)" class="text-yellow-500 hover:text-yellow-700">
                <i class="fas fa-edit"></i>
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const categories = ref([
    { id: '1', description: 'Electronics', status: 'active' },
    { id: '2', description: 'Furniture', status: 'inactive' }
  ]);
  
  const isFormVisible = ref(false);
  const formMode = ref('add'); // or 'edit'
  const form = ref({
    id: '',
    description: '',
    status: 'active'
  });
  
  function openForm(mode = 'add', category = null) {
    formMode.value = mode;
    if (mode === 'edit' && category) {
      form.value = { ...category };
    } else {
      form.value = { id: '', description: '', status: 'active' };
    }
    isFormVisible.value = true;
  }
  
  function saveCategory() {
    if (formMode.value === 'add') {
      categories.value.push({ ...form.value });
    } else if (formMode.value === 'edit') {
      const index = categories.value.findIndex(cat => cat.id === form.value.id);
      if (index !== -1) {
        categories.value[index] = { ...form.value };
      }
    }
    isFormVisible.value = false;
  }
  
  function viewCategory(category) {
    alert(`Viewing category: ${category.description}`);
  }
  
  function editCategory(category) {
    openForm('edit', category);
  }
  </script>
  
  <style scoped>
  /* Add any additional styling here */
  </style>
  