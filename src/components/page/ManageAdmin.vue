<template>
    <div class="table">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-date"></i> 系统管理</el-breadcrumb-item>
                <el-breadcrumb-item>管理系统账号</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="handle-box">
            <el-button type="primary" icon="delete" class="handle-del mr10" @click="delAll">批量删除</el-button>
            <el-select v-model="select_cate" placeholder="筛选" class="handle-select mr10">
                <el-option key="1" label="账号" value="账号"></el-option>
                <el-option key="2" label="姓名" value="姓名"></el-option>
            </el-select>
            <el-input v-model="select_word" placeholder="查询关键词" class="handle-input mr10"></el-input>
            <el-button type="primary" icon="search" @click="search">搜索</el-button>
        </div>
        <el-table :data="data" border style="width: 100%" ref="multipleTable" @selection-change="handleSelectionChange">
            <el-table-column type="selection" width="55"></el-table-column>
            <el-table-column prop="account" label="账号" sortable width="150">
            </el-table-column>
            <el-table-column prop="password" label="密码" width="120">
            </el-table-column>
            <el-table-column prop="name" label="姓名" >
            </el-table-column>
            <el-table-column label="操作" width="180">
                <template  slot-scope="scope">
                    <el-button size="small"
                            @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                    <el-button size="small" type="danger"
                            @click="handleDelete(scope.$index, scope.row,scope.row._id)">删除</el-button>
                </template>
            </el-table-column>
        </el-table>
        <div class="pagination">
            <el-pagination
                    @current-change ="handleCurrentChange"
                    layout="prev, pager, next"
                    :total="10">
            </el-pagination>
        </div>
        <!-- 确认删除对话框 -->
		<el-dialog title="请确认删除信息" :visible.sync="dialogVisible" width="30%" id="hhh">
      	<span>{{dialogMsg}}</span>
    	  <span slot="footer" class="dialog-footer">
    	    <el-button @click="dialogVisible=false">取 消</el-button>
    	    <el-button type="primary" @click="doDel">确 定</el-button>
    	  </span>
    </el-dialog>
    
    </div>
</template>
<style>
.el-dialog {
  width: 30%;
}
</style>

<script>
export default {
  data() {
    return {
      url: "./static/vuetable.json",
      tableData: [],
      cur_page: 1,
      multipleSelection: [],
      select_cate: "",
      select_word: "",
      del_list: [],
      is_search: false,
      
      dialogVisible: false,
      temDelRow: {},
      dialogMsg:""
    };
  },
  created() {
    this.getData();
  },
  computed: {
    data() {
      console.log('计算computed');
      const _this = this;
      console.log(_this.tableData);
      return _this.tableData.filter(function(d) {
        let is_del = false;
        for (let i = 0; i < _this.del_list.length; i++) {
          if (d.name === _this.del_list[i].name) {
            is_del = true;
            break;
          }
        }
        console.log(d);
        if (!is_del) {
          if (
            d.account.indexOf(_this.select_cate) > -1 &&
            (d.name.indexOf(_this.select_word) > -1 ||
              d.account.indexOf(_this.select_word) > -1)
          ) {
            return d;
          }
        }
      });
    }
  },
  methods: {
    handleCurrentChange(val) {
      this.cur_page = val;
      this.getData();
    },
    getData() {
      let _this = this;
      if (process.env.NODE_ENV === "development") {
        // _this.url = "/ms/table/list";
        _this.url = "/admins?page="+_this.cur_page;
      }
      _this.$axios.get(global.ApiUrl + "/admins").then(res => {
        console.log(res.data.data);
        _this.tableData = res.data.data;
      });
    },
    search() {
      this.is_search = true;
    },
    formatter(row, column) {
      return row.address;
    },
    filterTag(value, row) {
      return row.tag === value;
    },
    handleEdit(index, row) {
      this.$message("编辑第" + (index + 1) + "行");
    },
    // 确认删除提示框
    handleDelete(index, row) {
      this.dialogVisible = true;
      this.dialogMsg = `确认删除管理员：${row.name}`;
      this.temDelRow = row;
    },
    // 删除
    doDel() {
      this.dialogVisible = false;
      const _this = this;
      this.$axios
        .delete(global.ApiUrl + "/admin/" + this.temDelRow._id)
        .then(function(res) {
          console.log(res);
          if (res.data.code == "y") {
            _this.$message.success("删除成功~");
          } else {
            _this.$message.success("删除失败！");
          }
          // 刷新页面
          // _this.$router.go(0);
          // _this.$root.reload();
          // _this.$router.push({
          //   name: "manageadmin",
          //   query: { random: Math.random() }
          // });
          _this.getData();
        });
    },
    delAll() {
      const _this = this,
        length = _this.multipleSelection.length;
      let str = "";
      _this.del_list = _this.del_list.concat(_this.multipleSelection);
      for (let i = 0; i < length; i++) {
        str += _this.multipleSelection[i].name + " ";
      }
      _this.$message.error("删除了" + str);
      _this.multipleSelection = [];
    },
    handleSelectionChange(val) {
      this.multipleSelection = val;
    }
  }
};
</script>

<style scoped>
.handle-box {
  margin-bottom: 20px;
}
.handle-select {
  width: 120px;
}
.handle-input {
  width: 300px;
  display: inline-block;
}
</style>