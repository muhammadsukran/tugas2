<template>
  <div class="background">
    <img src="https://i.ibb.co/Rg7dRJG/image.png" />
  </div>
  <div class="container">
    <h1 :class="titleClass" :style="titleStyle">List Data</h1>
    <div class="row">
      <div class="col-5 table-container">
        <table class="table table-transparent">
          <thead>
            <tr>
              <th>#</th>
              <th>Name</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in items" :key="index">
              <td>{{ index + 1 }}</td>
              <td>{{ item.name }}</td>
              <td>
                <button class="btn btn-primary me-2"  @click="editItem(item)">Edit</button>
                <button class="btn btn-danger" @click="deleteItem(item)">Delete</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <div class="row mt-5"> 
      <div class="col-3 input-container">
        <h2 class="text-left">Add Data</h2>
        <div class="form-group mb-3">
          <input type="text" class="form-control input-transparent" v-model="newItem" placeholder="Enter new item">
        </div>
        <button class="btn btn-success" @click="addItem">Add</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Swal from 'sweetalert2';

const items = ref([
  { name: 'Mhd.Sukran' },
  { name: 'M.Taufik Kurahman' },
  { name: 'Fathur Rahman' },
]);

const titleClass = 'text-left'; // Mengubah nilai titleClass menjadi 'text-left'
const titleStyle = { color: 'blue' };

let editedItem = null;
const newItem = ref('');

const editItem = (item) => {
  editedItem = item;
  Swal.fire({
    title: 'Edit Item',
    input: 'text',
    inputValue: item.name,
    showCancelButton: true,
    confirmButtonText: 'Save',
    showLoaderOnConfirm: true,
    preConfirm: (value) => {
      if (!value.trim()) {
        Swal.showValidationMessage('Name is required');
      }
      return value;
    },
    allowOutsideClick: () => !Swal.isLoading()
  }).then((result) => {
    if (result.isConfirmed) {
      editedItem.name = result.value;
      Swal.fire('Success', 'Item updated successfully', 'success');
    }
  });
};

const deleteItem = (item) => {
  Swal.fire({
    title: 'Are you sure?',
    text: `You want to delete ${item.name}?`,
    icon: 'warning',
    showCancelButton: true,
    confirmButtonColor: '#d33',
    cancelButtonColor: '#3085d6',
    confirmButtonText: 'Delete'
  }).then((result) => {
    if (result.isConfirmed) {
      const index = items.value.indexOf(item);
      items.value.splice(index, 1);
      Swal.fire('Deleted!', 'Your item has been deleted.', 'success');
    }
  });
};

const addItem = () => {
  const itemName = newItem.value.trim();
  if (itemName) {
    items.value.push({ name: itemName });
    newItem.value = '';
    Swal.fire('Success', 'Item added successfully', 'success');
  } else {
    Swal.fire('Error', 'Please enter a valid item name', 'error');
  }
};
</script>

<style scoped>
.container{
  margin-top: 5%;
  padding-left: 30%;
  max-width: 100%;
  height: min-content;
  z-index: 1;
}
.background img{
  position: fixed;
  top: 0;
  left: 0;
  min-height: 90%;
  min-width: 1500px;
  width: 100%;
  height: 100%;
  overflow: hidden;
  z-index: -2;
  object-fit: cover;
  -webkit-background-size:cover; -moz-background-size:cover; -o-background-size:cover; background-size: cover;
  filter: brightness(0.8);
}
.table-transparent {
  background-color: transparent;
  color: white;
}
.input-transparent {
  background-color: transparent;
  border-color: white;
  color: white;
}
.input-container {
  background-color: transparent;
  padding: 5px;
  border-radius: 10px;
}
tr, th{
  background-color:gray;
}
tr, td{
  background-color: transparent;
  backdrop-filter: blur(5px);
}
</style>
