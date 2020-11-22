<template>
    <div>
        <el-button type="info" @click="logout">Log out</el-button>
    </div>
</template>
<script>
export default {
  methods: {
    logout () {
      console.log(this)
      window.sessionStorage.clear()
      this.$router.push('/login')
    }
  }
}
</script>
<style lang="less" scoped>
</style>
