<template>
  <div class="user-feedback">
    <div class="search">
      <div class="select-search">
        <el-input
          placeholder="请输入用户昵称/手机号"
          suffix-icon="el-icon-search"
          v-model="name">
        </el-input>
        <el-input
          class="chat-word"
          placeholder="请输入关键词"
          suffix-icon="el-icon-search"
          v-model="content">
        </el-input>
      </div>
      <el-button type="primary" @click="handleQueryClick">查询</el-button>
    </div>
    <div class="table">
      <el-table
        :data="tableData"
        style="width: 100%"
      >
        <el-table-column
          label="序号"
          type="index"
          width="50"
          align="center"
        >
        </el-table-column>
        <el-table-column
          prop="nickname"
          label="昵称"
          align="center"
        >
        </el-table-column>
        <el-table-column
          prop="phone"
          label="手机号"
          align="center"
        >
        </el-table-column>
        <el-table-column
          prop="type"
          label="板块"
          align="center"
          :formatter="typeFormatter"
        >
        </el-table-column>
        <el-table-column
          prop="content"
          label="反馈内容"
          align="center"
        >
        </el-table-column>
        <el-table-column
          prop="topic"
          label="主题"
          align="center"
        >
        </el-table-column>
        <el-table-column
          prop="createTimeString"
          label="反馈时间"
          align="center"
        >
        </el-table-column>
        <el-table-column
          fixed="right"
          label="操作"
          width="100">
          <template slot-scope="scope">
            <el-button @click="handleDeleteClick(scope.row)" type="text" size="small">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <div class="footer">
      <el-pagination
        background
        layout="total, sizes, prev, pager, next"
        :current-page="pageNum"
        :page-sizes="[10, 20, 30, 40]"
        :page-size="pageSize"
        :total="total"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
      >
      </el-pagination>
    </div>
  </div>
</template>
<script>
import {feedbackList,feedbackDelete} from "@/api/management";

export default {
  data() {
    return {
      name: '',
      content: '',
      tableData: [],
      pageNum: 1,
      pageSize: 10,
      total: 0,
    }
  },
  created() {
    this.queryFeedbackList()
  },
  methods: {
    async queryFeedbackList() {
      const params = {
        pageNum: this.pageNum,
        pageSize: this.pageSize,
        name: this.name,
        content: this.content
      }
      const res = await feedbackList(params)
      if(res.code===200) {
        this.tableData = res.rows
        this.total = res.total
      }
    },
    handleQueryClick() {
      this.queryFeedbackList()
    },
    async handleDeleteClick(row) {
      const params = {
        id: row.id
      }
      const res = await feedbackDelete(params)
      if(res.code===200) {
        this.$message("删除成功")
        this.queryFeedbackList()
      }
    },
    handleSizeChange(val) {
      this.pageSize = val
      this.queryFeedbackList()
    },
    handleCurrentChange(val) {
      this.pageNum = val
      this.queryFeedbackList()
    },
    typeFormatter(row) {
      switch (row.type) {
        case 0:
          return "意见反馈"
        case 1:
          return "广场"
        case 2:
          return "攻略"
      }
    }
  }
}
</script>
<style lang="scss" scoped>
.user-feedback {
  height: 100%;
  margin: 20px;

  .search {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    .select-search {
      display: flex;
      justify-content: space-between;
      align-items: center;
      .chat-word {
        margin: 0 10px;
      }
    }
  }

  .table {
    margin: 20px 0;
  }
}
</style>


