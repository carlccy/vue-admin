<template>
	<section>
    <el-col :span="24" class="toolbar" style="padding-bottom:0;">
      <el-form :inline="true" :model="formInline" class="demo-form-inline">
        <el-form-item>
          <el-input v-model="formInline.username" placeholder="请输入姓名"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="add">新增</el-button>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="search">查询</el-button>
        </el-form-item>
      </el-form> 
    </el-col>

    <el-table
      ref="multipleTable"
      :data="tableData3"
      tooltip-effect="dark"
      style="width: 100%"
      @selection-change="handleSelectionChange"
      v-loading="listLoading">
      <el-table-column
        type="selection"
        width="55">
      </el-table-column>
      <el-table-column
        label="日期"
        width="120"
        prop="birth"
        sortable>
      </el-table-column>
      <el-table-column
        prop="name"
        label="姓名"
        width="120">
      </el-table-column>
      <el-table-column
        prop="addr"
        label="地址"
        show-overflow-tooltip>
      </el-table-column>
      <el-table-column label="操作" width="150">
        <template slot-scope="scope">
          <el-button
            size="mini"
            @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
          <el-button
            size="mini"
            type="danger"
            @click="handleDelete(scope.$index, scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <div style="margin-top: 20px">
      <el-button @click="toggleSelection([tableData3[1], tableData3[2]])">切换第二、第三行的选中状态</el-button>
      <el-button @click="toggleSelection()">取消选择</el-button>
    </div>
    <el-col :span="24">
      <el-pagination
        background
        layout="total, prev, pager, next"
        :page-size="20"
        :total="total"
        @current-change="handleCurrentChange"
        style="float: right;">
      </el-pagination>
    </el-col>

    <el-dialog title="修改" :visible.sync="dialogFormVisible">
      <el-form :model="formData">
        <el-form-item label="姓名" :label-width="formLabelWidth">
          <el-input v-model="formData.name" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="生日" :label-width="formLabelWidth">
          <el-date-picker v-model="formData.birth" placeholder="选择日期"></el-date-picker>
        </el-form-item>
        <el-form-item label="地址" :label-width="formLabelWidth">
          <el-input v-model="formData.addr" auto-complete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
      </div>
    </el-dialog>
	</section>
</template>

<script>
import { getUserListPage } from "../../api/api";

export default {
  data() {
    return {
      formInline: {
        username: '',
        region: ''
      },
      tableData3: null,
      multipleSelection: [],
      page: 1,
      listLoading: false,
      total:0,
      dialogFormVisible: false,
      formData: {
        birth: null,
        name: '',
        addr: ''
      },
      formLabelWidth: '80px'
    };
  },
  methods: {
    search() {
      console.log('search!');
    },
    add() {
      console.log('add!')
      this.dialogFormVisible = true
    },
    toggleSelection(rows) {
      if (rows) {
        rows.forEach(row => {
          this.$refs.multipleTable.toggleRowSelection(row);
        });
      } else {
        this.$refs.multipleTable.clearSelection();
      }
    },
    handleSelectionChange(val) {
      this.multipleSelection = val;
    },
    handleEdit(index, row) {
      console.log(index, row);
    },
    handleDelete(index, row) {
      console.log(index, row);
    },
    getUsers() {
      let para = {
        page: this.page,
        // name: this.filters.name
      };
      //NProgress.start();
      this.listLoading = true;
      getUserListPage(para).then((res) => {
        this.total = res.data.total;
        // this.users = res.data.users;
        this.tableData3 = res.data.users;
        // console.log(res)
        this.listLoading = false;
        //NProgress.done();
      });
    },
    handleCurrentChange(val) {
      this.page = val
      let para = {
        page: this.page,
        // name: this.filters.name
      }
      this.listLoading = true;
      getUserListPage(para).then((res) => {
        this.total = res.data.total;
        // this.users = res.data.users;
        this.tableData3 = res.data.users;
        // console.log(res)
        this.listLoading = false;
        //NProgress.done();
      });
    }
  },
  mounted() {
    this.getUsers();
  }
};
</script>

<style scoped>

</style>