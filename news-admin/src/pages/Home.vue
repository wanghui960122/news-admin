<template>
  <div class="home">
    <el-container>
  <el-aside width="200px">
    <div class="logo">黑马头条</div>
    <el-menu
      background-color="#545c64"
      text-color="#fff"
      active-text-color="#ffd04b"
      router
      :default-active="$route.path"
      >
      <el-menu-item index="/post-list">
        <i class="el-icon-menu"></i>
        <span slot="title">文章列表</span>
      </el-menu-item>
      <el-menu-item index="/post-publish">
        <i class="el-icon-setting"></i>
        <span slot="title">发布文章</span>
      </el-menu-item>
    </el-menu>
  </el-aside>
  <el-container>
    <el-header>
      <img :src="$axios.defaults.baseURL + user.head_img" alt="">
      <div class="nickname">{{user.nickname}}</div>
      <div class="logout" @click="logout">退出</div>
    </el-header>
    <el-main>
      <router-view></router-view>
    </el-main>
  </el-container>
</el-container>
  </div>
</template>

<script>
export default {
  data () {
    return {
      user: JSON.parse(localStorage.getItem('user'))
    }
  },
  methods: {
    async logout () {
      try {
        await this.$confirm('确定退出系统吗？', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        })
        console.log('点击确定')
        this.$message.success('退出成功')
        this.$router.push('/logn')
        localStorage.removeItem('token')
        localStorage.removeItem('user')
      } catch {
        this.$message('操作取消')
      }
    }
  }
}
</script>

<style lang="scss" scoped>
  .home {
    position: absolute;
    width: 100%;
    height: 100%;
    .el-container {
      height: 100%;
    }
    .el-header {
    background-color: #B3C0D1;
    color: #333;
    text-align: center;
    line-height: 60px;
    display: flex;
    align-items: center;
    justify-content: flex-end;
  }
  .el-aside {
    background-color: #545c64;
    color: #333;
    text-align: center;
    .logo {
      height: 60px;
      line-height: 60px;
      background-color: #333;
      color: #fff;
      font-weight: 700;
      font-size: 30px;
    }
  }
  .el-main {
    background-color: #E9EEF3;
    color: #333;
  }
  img {
    width: 48px;
    height: 48px;
    border-radius: 50%;
  }
  .nickname {
    margin: 0 10px;
  }
  .logout {
    color: red;
  }
  }
</style>
