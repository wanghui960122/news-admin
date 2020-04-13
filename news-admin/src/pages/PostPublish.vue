<template>
  <div class="post-publish">
    <el-breadcrumb separator="/">
    <el-breadcrumb-item to="/">首页</el-breadcrumb-item>
  <el-breadcrumb-item>发布文章</el-breadcrumb-item>
  </el-breadcrumb>
  <el-form :model="form" label-width="80px">
      <el-form-item
      label="标题">
      <el-input v-model="form.title" placeholder="请输入文章标题"></el-input>
      </el-form-item>
      <el-form-item
      label="内容">
      <vue-editor v-model="form.content"></vue-editor>
      </el-form-item>
      <el-form-item
      label="栏目">
      <el-checkbox-group v-model="form.categories">
      <el-checkbox :label="item.id" v-for="item in categoryList" :key="item.id">{{item.name}}</el-checkbox>
    </el-checkbox-group>
      </el-form-item>
      <el-form-item label="封面">
        <el-upload
  :action="$axios.defaults.baseURL + '/upload'"
  :headers="headers"
  :on-success="handleSuccess"
  :file-list="form.cover"
  :on-remove="handleRemove"
  list-type="picture-card">
  <i class="el-icon-plus"></i>
</el-upload>
      </el-form-item>
      <el-form-item label="类型">
        <el-radio-group v-model="form.type">
      <el-radio :label="1">文章</el-radio>
      <el-radio :label="2">视频</el-radio>
    </el-radio-group>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="publish">发布</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import { VueEditor } from 'vue2-editor'
export default {
  components: {
    VueEditor
  },
  created () {
    this.getCategoryList()
    this.postId = this.$route.params.id
    console.log(this.postId)
    if (this.postId) {
      this.getPostDeatil(this.postId)
    }
  },
  methods: {
    async getCategoryList () {
      const res = await this.$axios.get('/category')
      console.log(res)
      this.categoryList = res.data.data
      this.categoryList = this.categoryList.filter(item => item.id !== 999)
    },
    async getPostDeatil (id) {
      const res = await this.$axios.get(`/post/${id}`)
      console.log(res)
      res.data.data.categories = res.data.data.categories.map(item => item.id)
      res.data.data.cover.forEach(item => {
        item.url = this.$fixUrl(item.url)
      })
      res.data.data.content = res.data.data.content.replace(/div/g, 'p')
      console.log(res.data.data.content)
      this.form = res.data.data
    },
    handleRemove (file) {
      this.form.cover = this.form.cover.filter(item => item.id !== file.id)
    },
    async publish () {
      console.log(this.form)
      const data = { ...this.form }
      data.categories = this.form.categories.map(item => {
        return {
          id: item
        }
      })
      let url
      if (this.postId) {
        url = `/post_update/${this.postId}`
      } else {
        url = '/post'
      }
      const res = await this.$axios.post(url, data, {
        headers: {
          Authorization: localStorage.getItem('token')
        }
      })
      console.log(res)
      this.$router.push('/post-list')
    },
    handleSuccess (res) {
      console.log(res)
      this.form.cover.push({
        id: res.data.id,
        url: this.$axios.defaults.baseURL + res.data.url
      })
      console.log(this.form.cover)
    }
  },
  data () {
    return {
      form: {
        title: '',
        content: '',
        categories: [],
        cover: [],
        type: 1
      },
      categoryList: [],
      postId: '',
      headers: {
        Authorization: localStorage.getItem('token')
      }
    }
  }
}
</script>

<style lang="scss" scoped>
  .el-form {
    margin-top: 20px;
  }
</style>
