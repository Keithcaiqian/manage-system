<template>
  <div class="goods-container">
    <!-- 面包屑导航 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>商品管理</el-breadcrumb-item>
      <el-breadcrumb-item>商品列表</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 卡片区域 -->
    <el-card>
      <el-row :gutter="20">
        <el-col :span="8">
          <el-input placeholder="请输入内容" v-model="goodsData.query" clearable @clear="getAllGoods()">
            <el-button slot="append" icon="el-icon-search" @click="searchGoods"></el-button>
          </el-input>
        </el-col>
        <el-col :span="4">
          <el-button type="primary" @click="addGoods">添加商品</el-button>
        </el-col>
      </el-row>
      <!-- 表格区域 -->
      <el-table :data="goods" style="width: 100%" border>
        <el-table-column type="index" label="No"></el-table-column>
        <el-table-column prop="goods_name" label="商品名称"></el-table-column>
        <el-table-column prop="goods_price" width="80px" label="商品价格（元）"></el-table-column>
        <el-table-column prop="goods_weight" width="80px" label="商品重量"></el-table-column>
        <el-table-column prop="add_time" width="200px" label="创建时间">
          <template slot-scope="scope">{{scope.row.add_time|capitalize}}</template>
        </el-table-column>
        <el-table-column label="操作" width="130px">
          <template slot-scope="scope">
            <el-button size="mini" type="primary" icon="el-icon-edit" ></el-button>
            <el-button size="mini" type="danger" icon="el-icon-delete" @click="deleteGoods(scope.row.goods_id)"></el-button>
          </template>
        </el-table-column>
      </el-table>
      <!-- page区域 -->
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="goodsData.pagenum"
        :page-sizes="[10, 30, 50, 100]"
        :page-size="goodsData.pagesize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
      ></el-pagination>
    </el-card>
  </div>
</template>
<script>
export default {
  data () {
    return {
      goodsData: {
        query: '',
        pagenum: 1,
        pagesize: 5
      },
      //   总商品数量
      total: 0,
      goods: []
      // page区域

    }
  },
  created () {
    this.getAllGoods()
  },
  methods: {
    async getAllGoods () {
      const { data: res } = await this.$axios.get('goods', {
        params: this.goodsData
      })
      this.total = res.data.total
      this.goods = res.data.goods
      console.log(res)
    },
    // 改变页码大小
    handleSizeChange (val) {
      this.goodsData.pagesize = val
      this.getAllGoods()
    },
    // 改变当前页码值
    handleCurrentChange (val) {
      this.goodsData.pagenum = val
      this.getAllGoods()
    },
    // 查询商品
    searchGoods () {
      this.getAllGoods()
    },
    // 删除商品
    async deleteGoods (id) {
      const result = await this.$MessageBox.confirm('此操作将永久删除该文件, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).catch(err => err)
      if (result === 'cancel') {
        return this.$Message.info('取消成功')
      } else {
        const { data: res } = await this.$axios.delete('goods/' + id)
        if (res.meta.status !== 200) {
          return this.$Message.error('删除失败')
        } else {
          this.$Message.success('删除成功')
          this.getAllGoods()
        }
      }
    },
    // 添加商品
    addGoods () {
      this.$router.push('/addGoods')
    }
  }
}
</script>
<style lang="scss" scoped>
.el-table {
  margin: 15px 0;
  font-size: 12px;
}
</style>
