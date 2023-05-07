<template>
  <el-card>
    <el-row :gutter="20" class="header">
      <el-col :span="7">
        <el-input
          :placeholder="$t('table.placeholder')"
          clearable
          v-model="queryForm.query"
        >
        </el-input>
      </el-col>
      <el-button type="primary" :icon="Search" @click="initGetUsersList">{{
        $t('table.search')
      }}</el-button>
      <el-button type="primary" @click="handleDialogValue">{{
        $t('table.adduser')
      }}</el-button>
    </el-row>

    <el-table type="index" :data="tableData" stripe style="width: 66%">
      <!-- <el-table-column type="index"></el-table-column> -->
      <el-table-column
        :width="item.width"
        :prop="item.prop"
        :label="$t(`table.${item.label}`)"
        v-for="(item, index) in options"
        :key="index"
      >
        <template v-slot="{ row }" v-if="item.prop === 'mg_state'">
          <el-switch v-model="row.mg_state" @change="changeState(row)" />
        </template>
        <template v-slot="{ row }" v-else-if="item.prop === 'create_time'">
          <!-- <el-switch v-model="row.mg_state" /> -->
          {{ $filters.filetrTimes(row.create_time) }}
        </template>
        <template #default v-else-if="item.prop === 'action'">
          <el-button type="primary" size="small" :icon="Edit"></el-button>
          <el-button type="warning" size="small" :icon="Setting"></el-button>
          <el-button type="danger" size="small" :icon="Delete"></el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      v-model:current-page="queryForm.pagenum"
      :page-sizes="[8, 16, 32]"
      :page-size="queryForm.pagesize"
      :background="background"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
    />
  </el-card>
  <Dialog
    v-model="dialogVisible"
    :dialogTitle="dialogTitle"
    v-if="dialogVisible"
  />
</template>

<script setup>
import { ref } from 'vue'
import { Search, Edit, Setting, Delete } from '@element-plus/icons-vue'
import { getUser, changeUserState } from '@/api/user'
import { options } from './options'
import { ElMessage } from 'element-plus'
import { useI18n } from 'vue-i18n'
import Dialog from './components/dialog.vue'
const i18n = useI18n()

const queryForm = ref({
  query: '',
  pagenum: 1,
  pagesize: 8
})
const total = ref(0)

const tableData = ref([])

const dialogVisible = ref(false)
const dialogTitle = ref('')

const initGetUsersList = async () => {
  const res = await getUser(queryForm.value)
  // console.log(res)
  total.value = res.total
  tableData.value = res.users
}

initGetUsersList()

const handleSizeChange = (pageSize) => {
  queryForm.value.pagenum = 1
  queryForm.value.pagesize = pageSize
  initGetUsersList()
}

const handleCurrentChange = (pageNum) => {
  queryForm.value.pagenum = pageNum
  initGetUsersList()
}

const changeState = async (info) => {
  await changeUserState(info.id, info.mag_state)
  ElMessage({
    message: i18n.t('message.updeteSuccess'),
    type: 'success'
  })
}

const handleDialogValue = () => {
  dialogTitle.value = '添加用户'
  dialogVisible.value = true
}
</script>

<style lang="scss" scoped>
.header {
  padding-bottom: 16px;
  box-sizing: border-box;

  :v-deep(.el-input__suffix) {
    align-items: center;
  }
}
</style>
