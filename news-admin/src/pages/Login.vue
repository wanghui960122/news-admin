<template>
  <div class="login">
    <el-form ref="form" :model="form" label-width="80px" class="from">
  <el-form-item label="账号">
    <el-input v-model="form.username" placeholder="请输入用户名" clearable></el-input>
  </el-form-item>
  <el-form-item label="密码">
    <el-input v-model="form.password" type="password" placeholder="请输入密码" show-password></el-input>
  </el-form-item>
  <el-form-item>
    <el-button type="primary" @click="login">登录</el-button>
    <el-button @click="reset">重置</el-button>
  </el-form-item>
</el-form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      form: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    async login () {
      if (!this.form.username.trim() || !this.form.password) {
        this.$message.warning('用户名或密码不能为空')
        return
      }
      const { data } = await this.$axios.post('/login', {
        username: this.form.username,
        password: this.form.password
      })
      console.log(data)
      if (data.statusCode === 200) {
        localStorage.setItem('token', data.data.token)
        localStorage.setItem('user', JSON.stringify(data.data.user))
        this.$message.success('登录成功')
        this.$router.push('/')
      } else {
        this.$message.error('用户名或密码错误')
      }
    },
    reset () {
      this.form.username = ''
      this.form.password = ''
    }
  }
}
</script>

<style lang="scss" scoped>
  .from {
    width: 600px;
    height: 250px;
    margin: 200px auto;
    border: 1px solid #ccc;
    padding: 100px 20px 0 0;
  }
</style>
