<template>
  <el-dialog
    :model-value="dialogVisible"
    :title="dialogTitle"
    width="30%"
    @close="handleClose"
  >
    <el-form ref="formRef" :model="form" label-width="70px" :rules="rules">
      <el-form-item label="用户名" prop="username">
        <el-input v-model="form.username" />
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input v-model="form.password" type="password" />
      </el-form-item>
      <el-form-item label="邮箱" prop="email">
        <el-input v-model="form.email" />
      </el-form-item>
      <el-form-item label="手机" prop="mobile">
        <el-input v-model="form.mobile" />
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="handleClose">Cancel</el-button>
        <el-button type="primary" @click="handleConfirm"> Confirm </el-button>
      </span>
    </template>
  </el-dialog>
</template>

<script setup>
import { defineEmits, ref, defineProps } from 'vue'
import { addUser } from '@/api/user'
import i18n from '@/i18n/'
import { ElMessage } from 'element-plus'

defineProps({
  dialogTitle: {
    type: String,
    default: '',
    required: true
  }
})

const emits = defineEmits(['update:modelValue'])
const handleClose = () => {
  emits('update:modelValue', false)
}

const handleConfirm = async () => {
  await addUser(form.value)
  ElMessage({
    message: i18n.global.t('message.addSuccess'),
    type: 'success'
  })
  handleClose()
}

const formRef = ref(null)
const form = ref({
  username: '',
  password: '',
  email: '',
  mobile: ''
})

const rules = ref({
  username: [
    {
      required: true,
      message: 'Please select Activity zone',
      trigger: 'blur'
    }
  ],
  password: [
    {
      required: true,
      message: 'Please select Activity zone',
      trigger: 'blur'
    }
  ],
  email: [
    {
      required: true,
      message: 'Please select Activity zone',
      trigger: 'blur'
    },
    {
      type: 'email',
      message: '请输入正确邮箱',
      trigger: ['blur', 'change']
    }
  ],
  mobile: [
    {
      required: true,
      message: 'Please select Activity zone',
      trigger: 'blur'
    }
  ]
})
</script>

<style lang="scss" scoped></style>
