<template>
  <div class="home">
    <h1>Home View</h1>
  </div>
  <el-row>
    <el-col :span="2" :offset="22">
      <div>
        <el-button type="primary" @click="dialogFormVisible = true">Add Post</el-button>
      </div>
    </el-col>
  </el-row>

  <Table/>

  <el-dialog v-model="dialogFormVisible" title="Shipping address">
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
        <el-button type="primary" @click="sendData"
          >Confirm</el-button
        >
      </span>
    </template>
  </el-dialog>
</template>


<script lang="ts" setup>
import Table from '@/components/Table.vue'
import { reactive, ref } from 'vue'

const dialogTableVisible = ref(false)
const dialogFormVisible = ref(false)
const formLabelWidth = '140px'

const form = reactive({
  id: '',
  name: '',
  body: '',
  
})

const sendData = () => {
  fetch('https://jsonplaceholder.typicode.com/posts', {
  method: 'POST',
  body: JSON.stringify({
    title: form.name,
    body: form.body,
    userId: form.id,
  }),
  headers: {
    'Content-type': 'application/json; charset=UTF-8',
  },
})
  .then((response) => response.json())
  .then((json) => console.log(json));
}

</script>

<style scoped>
  .el-button--text {
    margin-right: 15px;
  }
  .el-select {
    width: 300px;
  }
  .el-input {
    width: 300px;
  }
  .dialog-footer button:first-child {
    margin-right: 10px;
  }
  </style>
