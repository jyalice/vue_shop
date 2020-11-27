<template>
    <el-container class="home-container">
      <!-- Header area -->
      <el-header>
        <div>
          <img src="../assets/header.png" alt="">
          <span>E-commerce Background Management System</span>
        </div>
        <el-button type="info" @click="logout">Log out</el-button>
      </el-header>
      <el-container>
        <!-- Aside area on the left -->
        <el-aside :width="isCollapse ? '64px' : '200px'" >
          <div class="toggle-button" @click="toggleCollapse">|||</div>
          <!-- NavMenu area -->
          <el-menu background-color="#333744" text-color="#fff" active-text-color="#409bff" unique-opened :collapse="isCollapse" :collapse-transition="false" router :default-active="activePath">
            <!-- 1st-Level Menu -->
            <el-submenu :index="item.id + ''" v-for="item in menulist" :key="item.id">
              <template slot="title">
                <i :class="iconObj[item.id]"></i>
                <span>{{item.authName}}</span>
              </template>
                <!-- 2nd-Level Menu -->
                <el-menu-item :index="'/' + subItem.path" v-for="subItem in item.children" :key="subItem.id" @click="saveNavState('/' + subItem.path)">
                  <template slot="title">
                    <i class="el-icon-menu"></i>
                    <span>{{subItem.authName}}</span>
                  </template>
                </el-menu-item>
            </el-submenu>
          </el-menu>
        </el-aside>
        <!-- Main area on the right -->
        <el-main>
          <router-view></router-view>
        </el-main>
      </el-container>
    </el-container>
</template>
<script>
export default {
  data () {
    return {
      menulist: [],
      iconObj: {
        125: 'iconfont icon-user',
        103: 'iconfont icon-tijikongjian',
        101: 'iconfont icon-shangpin',
        102: 'iconfont icon-danju',
        145: 'iconfont icon-baobiao'
      },
      isCollapse: false,
      activePath: ''
      // activePath: window.sessionStorage.getItem('activePath')
    }
  },
  created () {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout () {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    // save the active state
    saveNavState (activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.active = activePath
    },
    // get all menus
    async getMenuList () {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menulist = res.data
    },
    // click to toggle folding and unfolding
    toggleCollapse () {
      this.isCollapse = !this.isCollapse
    }
  }
}
</script>
<style lang="less" scoped>
.home-container{
  height: 100%;
}
.el-header{
  background-color: #373D41;
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  align-items: center;
  color: #fff;
  font-size: 20px;
  > div {
    display: flex;
    align-items: center;
    span{
      margin-left: 15px;
    }
  }
}
.el-aside{
  background-color: #333744;
  .el-menu{
    border-right: none;
  }
}
.el-main{
  background-color: #eaedf1;
}
.iconfont{
  margin-right: 10px;
}
.toggle-button{
  background-color: #4a5064;
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  text-align: center;
  letter-spacing: 0.2em;
  cursor: pointer;
}
</style>
