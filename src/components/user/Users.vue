<template>
  <div>
    <!-- Breadcrumb area -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">homepage</el-breadcrumb-item>
      <el-breadcrumb-item>user management</el-breadcrumb-item>
      <el-breadcrumb-item>user list</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- Card area -->
    <el-card>
      <!-- Input area to search for and/or add items -->
      <el-row :gutter="20">
        <el-col :span="8">
          <el-input placeholder="Please input" v-model="queryInfo.query" clearable @clear="getUserList">
            <el-button slot="append" icon="el-icon-search" @click="getUserList"></el-button>
          </el-input>
        </el-col>
        <el-col :span="4">
          <el-button type="primary">Add the user</el-button>
        </el-col>
      </el-row>
      <!-- User list area -->
      <el-table :data="userlist" border stripe>
        <el-table-column label="#" type="index">
        </el-table-column>
        <el-table-column label="Name" prop="username">
        </el-table-column>
        <el-table-column label="Email" prop="email">
        </el-table-column>
        <el-table-column label="Mobile" prop="mobile">
        </el-table-column>
        <el-table-column label="Role" prop="role_name">
        </el-table-column>
        <el-table-column label="State">
          <template slot-scope="scope">
            <el-switch v-model="scope.row.mg_state" @change="userStateChanged(scope.row)">
            </el-switch>
          </template>
        </el-table-column>
        <el-table-column label="Operation" width="180">
          <!-- <template slot-scope="scope"> -->
          <template>
            <!-- button for editing items-->
            <el-button type="primary" icon="el-icon-edit" size="mini"></el-button>
            <!-- button for deleting items -->
            <el-button type="danger" icon="el-icon-delete" size="mini"></el-button>
            <!-- button for setting roles -->
             <el-tooltip effect="dark" content="setting roles" placement="top" :enterable="false">
              <el-button type="warning" icon="el-icon-setting" size="mini"></el-button>
            </el-tooltip>
          </template>
        </el-table-column>
      </el-table>
      <!-- Pagination area -->
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page.sync="queryInfo.pagenum"
        :page-sizes="[1, 2, 5, 10]"
        :page-size="queryInfo.pagesize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total">
      </el-pagination>
    </el-card>
  </div>
</template>
<script>
export default {
  data () {
    return {
      queryInfo: {
        query: '',
        // current page number
        pagenum: 1,
        // options of item count per page
        pagesize: 2
      },
      userlist: [],
      total: 0
    }
  },
  created () {
    this.getUserList()
  },
  methods: {
    async getUserList () {
      const { data: res } = await this.$http.get('users', { params: this.queryInfo })
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.userlist = res.data.users
      this.total = res.data.total
    },
    handleSizeChange (newSize) {
      this.queryInfo.pagesize = newSize
      this.getUserList()
    },
    handleCurrentChange (newPage) {
      this.queryInfo.pagenum = newPage
      this.getUserList()
    },
    async userStateChanged (userinfo) {
      const { data: res } = await this.$http.put(`/users/${userinfo.id}/state/${userinfo.mg_state}`)
      if (res.meta.status !== 200) {
        userinfo.mg_state = !userinfo.mg_state
        return this.$message.error(res.meta.msg)
      }
      this.$message.success('Update succeeds')
    }
  }
}
</script>
<style lang="less" scoped>
</style>
