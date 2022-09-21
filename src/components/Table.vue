<template>
<div v-if="tableData" >
  <el-table :data="tableData" style="width: 100%">
        <el-table-column prop="id" label="ID" width="180" />
        <el-table-column prop="title" label="Title" width="180" />
        <el-table-column prop="body" label="Body" />
        <el-table-column align="right">

        <template #default="scope">
            <el-button size="small" @click="handleEdit(scope.$index, scope.row)">Edit</el-button>

            <RouterLink :to="{name: 'commentdetails', params: {id: scope.row['id']}}">
            <el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">Delete</el-button>
            </RouterLink>
        </template>
        </el-table-column>
  </el-table>
</div>

<div v-else>
    <h1>Loading ...</h1>
</div>
</template>

<script lang="ts" setup>
import {  ref, onMounted } from 'vue'
import { RouterLink, RouterView} from 'vue-router'

const tableData = ref([])
const x = ref('')

onMounted(() => {
  fetchData();
})

const fetchData = () => {
  fetch("https://jsonplaceholder.typicode.com/posts")
    .then((response) => response.json())
    .then((data) => tableData.value = data)
    .catch(error => {
        console.log(error);
    });
}

const handleEdit = (index: number, row) => {
//   console.log(row['id'])
  console.log("edit button clicked");

}

const handleDelete = (index: number, row) => {
  console.log("delete button clicked");
}



</script>