<template>
  <div>
    <!-- 遍历data中的item,取出item中的_id(此id自动生成)，name（数据库中定义）-->
    <el-table :data="items">
      <el-table-column label="ID" width="240" prop="_id"></el-table-column>
      <el-table-column label="分类名称" prop="name"></el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
            <el-button type="text" @click="$router.push(`/ads/edit/${scope.row._id}`)">编辑</el-button>
             <el-button type="text" @click="remove(scope.row)">删除</el-button>
        </template>
       
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data(){
    return {
      items:[]
    }
  },
  methods:{
    async list(){
      const res = await this.$http.get('rest/ads')
      this.items = res.data
    },
    remove(row){
       this.$confirm(`此操作将永久删除${row.name}`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(async () => {
          const res = await this.$http.delete(`rest/ads/${row._id}`)
          console.log(res)
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
          this.list()
        })
    }
  },
  created(){
    this.list()
  }
}
</script>
