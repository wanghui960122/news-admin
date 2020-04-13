<template>
  <div class="post-list">
    <el-breadcrumb separator="/">
  <el-breadcrumb-item to="/">首页</el-breadcrumb-item>
  <el-breadcrumb-item>文章列表</el-breadcrumb-item>
</el-breadcrumb>
<el-table
      :data="postlist"
      style="width: 100%">
      <el-table-column
      type="index"
      label="序号"
      width="50"
      :index="indexMethod">
    </el-table-column>
      <el-table-column
        prop="title"
        label="标题"
        width="500">
      </el-table-column>
      <el-table-column
        prop="user.nickname"
        label="作者"
        width="180">
      </el-table-column>
      <el-table-column
        prop="create_date"
        label="创建时间">
      </el-table-column>
      <el-table-column
        prop="cover[0].url"
        label="封面">
         <template v-slot="{row}">
        <img :src="$fixUrl(row.cover[0].url)" alt="">
      </template>
      </el-table-column>
      <el-table-column
        label="操作"
        width=220>
        <template v-slot="{row}">
          <el-button @click="edit(row.id)">编辑</el-button>
        <el-button type="danger">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
    layout="total, sizes, prev, pager, next, jumper"
    :total="total"
    :page-size="pageSize"
    :current-page="pageIndex"
    @current-change="handleCurrentChange"
    :page-sizes="[3,4,5,6]"
    @size-change="handleSizesChange"
    background>
  </el-pagination>
  </div>
</template>

<script>
export default {
  data () {
    return {
      postlist: [],
      pageIndex: 1,
      pageSize: 4,
      total: 0
    }
  },
  created () {
    this.getPostlist()
  },
  methods: {
    async getPostlist () {
      const res = await this.$axios.get('/post', {
        params: {
          pageIndex: this.pageIndex,
          pageSize: this.pageSize
        }
      })
      this.postlist = res.data.data
      this.total = res.data.total
    },
    indexMethod (index) {
      return (this.pageIndex - 1) * this.pageSize + index + 1
    },
    handleCurrentChange (val) {
      console.log(val)
      this.pageIndex = val
      this.getPostlist()
    },
    handleSizesChange (val) {
      this.pageIndex = 1
      this.pageSize = val
      this.getPostlist()
    },
    edit (id) {
      this.$router.push({
        name: 'post-publish',
        params: {
          id
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
  .post-list {
    .el-table {
      margin-top: 10px;
      img {
        width: 150px;
        height: 100px;
        object-fit: cover;
      }
    }
  }
</style>
