<template>
  <el-dropdown>
    <span class="el-dropdown-link">
      <el-avatar shape="square" :size="40" :src="squareUrl" />
    </span>
    <template #dropdown>
      <el-dropdown-menu>
        <el-dropdown-item @click="logout">退出</el-dropdown-item>
      </el-dropdown-menu>
    </template>
  </el-dropdown>
</template>

<script setup>
import { ref } from 'vue'
import { useStore } from 'vuex'
const store = useStore()
const squareUrl = ref(
  'https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fsafe-img.xhscdn.com%2Fbw1%2Fc24dbeff-1cfc-4f66-a56e-430c9d2e5100%3FimageView2%2F2%2Fw%2F1080%2Fformat%2Fjpg&refer=http%3A%2F%2Fsafe-img.xhscdn.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1685950530&t=57baf8a52c0b72c5085f6f2dc4d6c9c5'
)

const logout = () => {
  store.dispatch('app/logout')
}
</script>

<style lang="scss" scoped>
//处理退出竖向问题
// :deep(.el-dropdown-menu__item) {
//   white-space: inherit;
// }
</style>
