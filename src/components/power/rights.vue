<template>
  <div>
    <!-- 面包屑导航区 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>权限管理</el-breadcrumb-item>
      <el-breadcrumb-item>权限列表</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 卡片视图 -->
     <el-card class="box-card">
       <el-table
           :data="rightsList"
           border
           style="width: 100%"
           :stripe="true">
           <el-table-column type="index" label="Id"></el-table-column>
           <el-table-column
             prop="authName"
             label="权限名称">
           </el-table-column>
           <el-table-column
             prop="path"
             label="路径">
           </el-table-column>
           <el-table-column label="权限等级">
             <template slot-scope="scope">
               <el-tag :type="rightsRank[scope.row.level].type">{{rightsRank[scope.row.level].name}}</el-tag>
             </template>
           </el-table-column>
         </el-table>
         <el-pagination
               @size-change="handleSizeChange"
               @current-change="handleCurrentChange"
               :current-page="queryInfo.pagenum"
               :page-sizes="[2, 4, 6, 10]"
               :page-size="queryInfo.pageszie"
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
      rightsList: [],
      rightsRank: [
        { name: '一级', type: 'success' },
        { name: '二级', type: 'danger' },
        { name: '三级', type: 'warning' }
      ],
      queryInfo: {
        pageszie: 6,
        pagenum: 2
      },
      total: 0
    }
  },
  created () {
    this.getRightsList()
  },
  methods: {
    async getRightsList () {
      const { data: res } = await this.$request.get('rights/list')
      if (res.meta.status !== 200) { return this.$message.error('获取权限列表失败') }
      this.rightsList = res.data
      this.total = res.data.length
    },
    handleSizeChange (pageSize) {},
    handleCurrentChange (pageNum) {}
  }
}
</script>

<style lang="less" scoped="scoped">
</style>
