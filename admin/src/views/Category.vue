<template>
  <div class="about">
    <h1>分类列表</h1>
    <el-table :data="items" style="width: 100%">
      <el-table-column prop="_id" label="ID" width="240">
      </el-table-column>
      <el-table-column prop="parent.name" label="上级分类" width="180">
        </el-table-column>
      <el-table-column prop="name" label="分类名称" width="100">
      </el-table-column>
      <el-table-column fixed="right" label="操作" width="180">
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="$router.push(`/categories/edit/${scope.row._id}`)">编辑</el-button>
          <el-button type="text" size="small" @click="remove(scope.row)">删除</el-button>
        </template>
      </el-table-column>

    </el-table>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        items: []
      }
    },
    methods: {
      async fetch() {
        const res = await this.$http.get('rest/categories');// 注意await
        this.items = res.data;
      },
      handleClick() {

      },
      async remove(row) {
        this.$confirm(`是否确认要删除分类"${row.name}"`, '删除提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(async () => {
          const res = await this.$http.delete(`rest/categories/${row._id}`)
          if (res.data.success){
            this.$message({
              type: 'success',
              message: '删除成功!'
            });
            this.fetch()
          }
           
        });
      }
    },
    created() {
      this.fetch();
    }
  }</script>