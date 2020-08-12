<template>
  <div class="table">
    {{id}}
    <div class="crumbs">
      <el-breadcrumb separator="/">
        <el-breadcrumb-item>
          <i class="el-icon-tickets"></i> 收藏信息
        </el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="container">
      <div class="handle-box">
        <el-button type="primary" size="mini" class="handle-del mr10" @click="delAll">批量删除</el-button>
        <el-input v-model="select_word" size="mini" placeholder="筛选关键词" class="handle-input mr10"></el-input>
      </div>
      <el-table
        :data="tableData"
        size="mini"
        border
        style="width: 100%"
        ref="multipleTable"
        @selection-change="handleSelectionChange"
      >
        <el-table-column type="selection" width="50"></el-table-column>
        <el-table-column prop="name" label="歌手-歌曲"></el-table-column>
        <el-table-column label="操作" width="85">
          <template slot-scope="scope">
            <el-button size="mini" type="danger" @click="handleDelete(scope.row)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>

    <!-- 删除提示框 -->
    <el-dialog title="提示" :visible.sync="delVisible" width="300px" center>
      <div class="del-dialog-cnt" align="center">删除不可恢复，是否确定删除？</div>
      <span slot="footer" class="dialog-footer">
        <el-button size="mini" @click="delVisible = false">取 消</el-button>
        <el-button type="primary" size="mini" @click="deleteRow">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import { mixin } from '../mixins'

export default {
  name: 'collect-page',
  mixins: [mixin],
  props: ['id'],
  data () {
    return {
      tableData: [
        {
          name: '测试'
        }
      ], // 记录歌曲，用于显示
      tempDate: [], // 记录歌曲，用于搜索时能临时记录一份歌曲列表
      tempId: [], // 记录列表中歌曲的id
      multipleSelection: [], // 记录要删除的歌曲
      delVisible: false, // 显示删除框
      select_word: '', // 记录输入框输入的内容
      idx: -1 // 记录当前要删除的歌曲
    }
  },
  watch: {
    select_word: function () {
      if (this.select_word === '') {
        this.tableData = this.tempDate
      } else {
        this.tableData = []
        for (let item of this.tempDate) {
          if (item.name.includes(this.select_word)) {
            this.tableData.push(item)
          }
        }
      }
    }
  },
  methods: {
    // 删除一首歌曲
    deleteRow () {
      this.notify('删除成功', 'success')
      this.delVisible = false
    }
  }
}
</script>

<style scoped>
.handle-box {
  margin-bottom: 20px;
}

.handle-input {
  width: 300px;
  display: inline-block;
}
</style>
