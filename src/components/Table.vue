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
            <el-button size="small" type="primary">View</el-button>
            </RouterLink>
        </template>
        </el-table-column>
  </el-table>
</div>
<div v-else>
    <h1>Loading ...</h1>
</div>
  <el-dialog v-model="dialogFormVisible" title="Edit Post">
    <el-form :model="form">
      <el-form-item label="ID" :label-width="formLabelWidth">
        <el-input v-model="form.id" autocomplete="off" />
      </el-form-item>
      <el-form-item label="Title" :label-width="formLabelWidth">
        <el-input v-model="form.name" autocomplete="off" />
      </el-form-item>
      <el-form-item label="Body" :label-width="formLabelWidth">
        <el-input v-model="form.body" autocomplete="off" />
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="dialogFormVisible = false">Cancel</el-button>
        <el-button type="primary" @click="updateData"
          >Confirm</el-button
        >
      </span>
    </template>
  </el-dialog>
</template>

<script lang="ts" setup>
import { RouterLink, RouterView} from 'vue-router'
import { reactive, ref, onMounted } from 'vue'
import { ElMessage } from 'element-plus'

const tableData = ref([])
const x = ref('')

//Dialog
const dialogTableVisible = ref(false)
const dialogFormVisible = ref(false)
const formLabelWidth = '140px'

const form = reactive({
  id: '',
  name: '',
  body: '',
  
})

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

// const updateData = () => {
//   fetch('https://jsonplaceholder.typicode.com/posts/'+ form.id, {
//   method: 'PUT',
//   body: JSON.stringify({
//     id: form.id,
//     title: form.name,
//     body: form.body,
//     userId: 1,
//   }),
//   headers: {
//     'Content-type': 'application/json; charset=UTF-8',
//   },
// })
//   .then((response) => response.json())
//   .then((json) => console.log(json))
//   .catch(error => {
//     console.log(error);
//   });
// }

const updateData = () => {
  fetch('https://jsonplaceholder.typicode.com/posts/'+ form.id, {
  method: 'PUT',
  body: JSON.stringify({
    id: form.id,
    title: form.name,
    body: form.body,
    userId: 1,
  }),
  headers: {
    'Content-type': 'application/json; charset=UTF-8',
  },
})
  .then(parseJSON) 
  .then(checkStatus)
  .then((data) => console.log(data))
  .catch(error => console.log(error));
}

const handleEdit = (index: number, row) => {
  dialogFormVisible.value = true;
  form.id = row['id'];
  form.name = row['title'];
  form.body = row['body'];
}

const checkStatusCode = (status) => {
  if(status >= 200 && status < 300){
    ElMessage({
    message: 'Data Updated.',
    type: 'success',
  })

  dialogFormVisible.value = false;

  }
  else {
    ElMessage.error('Error Updating Data. Status Code: ' +status);
  }
    
}

function checkStatus({data, status}) {

    if (status >= 200 && status < 300) {
        checkStatusCode(status);
        return data;
    }
    else {
        // const error = new Error(response.statusText);
        checkStatusCode(status);
        const error = new Error("Something went wrong");
        // error.response = response;
        error.data = data;

        throw error;
    }

}

function parseJSON(response) {
    return response.json().then(json => {
          return {
                   data: json,
                   status: response.status  
                 }
    })         
}  



</script>

<style>
a{
  text-decoration: none;
}

</style>