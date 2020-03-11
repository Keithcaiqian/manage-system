<template>
  <div class="params-container">
    <!-- 面包屑导航 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>商品管理</el-breadcrumb-item>
      <el-breadcrumb-item>分类参数</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 卡片区域 -->
    <el-card>
      <el-alert show-icon title="注意：只允许为第三级分类设置相关参数！" type="warning"></el-alert>
      <el-row>
        <span>选择商品分类：</span>
        <!-- 级联选择器 -->
        <el-cascader
          clearable
          v-model="selCate"
          :options="allCates"
          :props="{ expandTrigger: 'hover',label: 'cat_name',value: 'cat_id',children:'children' }"
          @change="cateChange"
        ></el-cascader>
        <el-tabs v-model="activeName" @tab-click="handleClick">
          <el-tab-pane label="动态参数" name="first">
              <el-button :disabled="isShowBtn" type="primary">添加参数</el-button>
          </el-tab-pane>
          <el-tab-pane label="静态参数" name="second">
              <el-button :disabled="isShowBtn" type="primary">添加属性</el-button>
          </el-tab-pane>
        </el-tabs>
      </el-row>
    </el-card>
  </div>
</template>
<script>
export default {
  data () {
    return {
      allCates: [],
      //   级联选择器
      selCate: [],
      activeName: 'first'
    }
  },
  created () {
    this.getAllCate()
  },
  methods: {
    async getAllCate () {
      const { data: res } = await this.$axios.get('categories', {
        params: {}
      })
      this.allCates = res.data
      console.log(this.allCates)
    },
    cateChange () {
      if (this.selCate.length !== 3) {
        this.selCate = []
        return
      }
      console.log(this.selCate)
    },
    handleClick (tab, event) {
      console.log(tab, event)
    }
  },
  computed: {
    isShowBtn () {
      if (this.selCate.length !== 3) {
        return true
      } else {
        return false
      }
    }
  }
}
</script>
<style lang="scss" scoped>
.el-alert {
  margin-bottom: 15px;
}
.el-cascader{
    margin-bottom: 15px;
}
</style>
