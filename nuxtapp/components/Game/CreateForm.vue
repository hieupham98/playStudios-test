<template>
  <el-form
    ref="ruleFormRef"
    style="max-width: 600px"
    :model="ruleForm"
    :rules="rules"
    label-width="auto"
    class="demo-ruleForm"
    :size="formSize"
    status-icon
  >
    <el-form-item label="Game" prop="name">
      <el-input v-model="ruleForm.name" />
    </el-form-item>
    <el-form-item label="Image" prop="imageUrl">
      <el-input v-model="ruleForm.imageUrl" />
    </el-form-item>
    <el-form-item label="Description" prop="desc">
      <el-input v-model="ruleForm.desc" type="textarea" />
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="submitForm(ruleFormRef)">
        Create
      </el-button>
      <el-button @click="resetForm(ruleFormRef)">Reset</el-button>
    </el-form-item>
  </el-form>
</template>

<script lang="ts" setup>
import { reactive, ref } from 'vue'
import type { ComponentSize, FormInstance, FormRules } from 'element-plus'


interface RuleForm {
  id: number
  name: string
  desc: string
  vote: number
  imageUrl: string
}


const ideas = ref([])

onMounted(() => {
  const savedIdeas = localStorage.getItem('game-ideas');

  if (savedIdeas) {
    ideas.value = JSON.parse(savedIdeas);
  }

})

const formSize = ref<ComponentSize>('default')
const ruleFormRef = ref<FormInstance>()
const ruleForm = reactive<RuleForm>({
  id: Date.now(),
  name: '',
  desc: '',
  vote: 0,
  imageUrl: '',
})


const checkNameExist = (rule: any, value: any, callback: any) => {

  if(ideas.value.length > 0) {
    const isDuplicate = ideas.value.some(item => item.name === value);
    if(isDuplicate) {
      callback(new Error('Game idea already exists'))

    } else {
      callback()
    }
  } else {
    callback()
  }


  // setTimeout(() => {
  //   if (!Number.isInteger(value)) {
  //     callback(new Error('Please input digits'))
  //   } else {
  //     if (value < 18) {
  //       callback(new Error('Age must be greater than 18'))
  //     } else {
  //       callback()
  //     }
  //   }
  // }, 1000)
}


const rules = reactive<FormRules<RuleForm>>({
  name: [
    { required: true, message: 'Please input Activity name', trigger: 'blur' },
    { min: 3, max: 255, message: 'Length should be 3 to 255', trigger: 'blur' },
    { validator: checkNameExist, trigger: 'blur' },

  ],
  desc: [
    { required: true, message: 'Please input activity form', trigger: 'blur' },
  ],
})

const submitForm = async (formEl: FormInstance | undefined) => {
  if (!formEl) return
  await formEl.validate((valid, fields) => {
    if (valid) {
        // Add new idea
      ideas.value.push({ ...ruleForm });


      localStorage.setItem('game-ideas', JSON.stringify(ideas.value));
      ElMessage({
        message: 'Create succesfully.',
        grouping: true,
        type: 'success',
      })
    } else {
      ElMessage({
        message: 'Something went wrong.',
        grouping: true,
        type: 'error',
      })
    }
  })
}

const resetForm = (formEl: FormInstance | undefined) => {
  if (!formEl) return
  formEl.resetFields()
}

const options = Array.from({ length: 10000 }).map((_, idx) => ({
  value: `${idx + 1}`,
  label: `${idx + 1}`,
}))
</script>
