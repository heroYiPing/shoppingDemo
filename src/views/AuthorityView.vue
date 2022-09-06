<template>
  <div>
    <el-tree
      ref="treeRef"
      :data="list"
      show-checkbox
      node-key="roleId"
      :default-checked-keys="authority"
      :check-strictly="true"
      :props="{
        children: 'roleList',
        label: 'name'
      }"
    />
    <el-button @click="changeAuthority">确认修改</el-button>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, reactive, toRefs } from 'vue'
import { useRoute } from 'vue-router'
import { InitData } from '../type/authority'
import { geAuthorityList } from '../request/api'
export default defineComponent({
  setup() {
    const route = useRoute()
    console.log(route)
    const params: any = route.params
    const data = reactive(new InitData(params.id, params.authority))
    onMounted(() => {
      geAuthorityList().then((res) => {
        console.log(res)
        data.list = res.data
      })
    })
    const changeAuthority = () => {
      console.log(
        data.treeRef.getCheckedKeys().sort(function (a: number, b: number) {
          return a - b
        })
      )
      // 传给后台进行修改
    }
    return { ...toRefs(data), changeAuthority }
  }
})
</script>

<style scoped></style>
