<template>
  <el-form
    ref="ruleFormRef"
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
    <!-- <el-form-item label="Image" prop="imageUrl">
      <el-input v-model="ruleForm.imageUrl" />
    </el-form-item> -->
    <el-form-item label="Description" prop="desc">
      <el-input v-model="ruleForm.desc" type="textarea" />
    </el-form-item>
    <el-row class="justify-center">
      <el-button type="primary" @click="submitForm(ruleFormRef)">
        <Icon name="lucide:check" class="mr-2"/>
        Create
      </el-button>
      <el-button @click="resetForm(ruleFormRef)">
        <Icon name="lucide:x" class="mr-2" />
        Reset
      </el-button>
    </el-row>
  </el-form>
</template>

<script lang="ts" setup>
  import { reactive, ref } from 'vue'
  import type { ComponentSize, FormInstance, FormRules } from 'element-plus'
  const router = useRouter()

  interface RuleForm {
    id: number
    name: string
    desc: string
    vote: number
    imageUrl: string
    user: string
  }

  const ideas = ref([])
  const currentUser = ref('')

  const formSize = ref<ComponentSize>('default')
  const ruleFormRef = ref<FormInstance>()
  const ruleForm = reactive<RuleForm>({
    id: Date.now(),
    name: '',
    desc: '',
    vote: 0,
    user: '',
    imageUrl: '',
  })

  onMounted(() => {
    const savedIdeas = localStorage.getItem('game-ideas');

    if (savedIdeas) {
      ideas.value = JSON.parse(savedIdeas);
    }

    const user = localStorage.getItem('current-user');

    if (user) {
        currentUser.value = user;
    }
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
        if (currentUser.value) {
          ruleForm.user = currentUser.value

          ideas.value.push({ ...ruleForm });


          localStorage.setItem('game-ideas', JSON.stringify(ideas.value));

          router.push({path: '/'})
          ElMessage({
            message: 'Create succesfully.',
            grouping: true,
            type: 'success',
          })
        } else {
          ElMessage({
            message: 'Please login',
            grouping: true,
            type: 'error',
          })
        }
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
