<template>
  <div class="table">
    <div class="handle-box">
      <el-input v-model="searchKey" placeholder="筛选关键词" class="handle-input mr10"></el-input>
      <el-button type="primary" icon="search" @click="search">搜索</el-button>
    </div>
    <el-table :data="questions" border ref="multipleTable" @selection-change="handleSelectionChange">

      <el-table-column prop="id" label="编号" width="65">
      </el-table-column>
      <el-table-column prop="question" label="问题">
        <template scope="scope">
          <a class="article-title" @click="editQuestion(scope.row)">{{ scope.row.question }}</a>
        </template>
      </el-table-column>
      <el-table-column prop="correct" label="正确答案" width="95">
      </el-table-column>
      <el-table-column prop="majorID" label="专业" width="70">
      </el-table-column>
      <el-table-column prop="uName" label="发布人" width="85">
      </el-table-column>
      <el-table-column prop="toAnswer" label="提示" width="85">
      </el-table-column>
      <el-table-column label="操作" width="150">
        <template scope="scope">
          <el-popover ref="deleteConfirm" placement="top" width="160" v-model="scope.row.confirmDeleteVisible">
            <p>确定删除吗？</p>
            <div style="text-align: right; margin: 0">
              <el-button size="mini" type="text" @click="deleteCalcel">取消</el-button>
              <el-button type="primary" size="mini" @click="deleteSubmit">确定</el-button>
            </div>
          </el-popover>
          <el-button size="small" type="primary" v-if="review" @click="handleReview(scope.$index, scope.row)">再审核</el-button>

          <el-button size="small" type="primary" v-if="!review" @click="handlePassed(scope.$index, scope.row)">通过</el-button>
          <el-button size="small" type="danger" v-if="!review" v-popover:deleteConfirm @click="handleDelete(scope.$index, scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <div class="pagination">
      <el-pagination @current-change="handleCurrentChange" layout="prev, pager, next" :total="totalQuestion" :page-size="perPageQuestions">
      </el-pagination>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      perPageQuestions: 10,
      totalQuestion: 0,
      questions: [],
      QuestionsBack: [],
      tableData: [],
      curPage: "",
      del_list: [],
      searchKey: "",
      currentDeleteItem: {},
      confirmDeleteVisible: false
    };
  },
  methods: {
    deleteCalcel() {
      console.log(this.currentDeleteItem);
      this.questions[this.currentDeleteItem.index].confirmDeleteVisible = false;
      this.currentDeleteItem = {};
      console.log("取消");
    },
    deleteSubmit() {
      let id = this.currentDeleteItem.id;
      const that = this;
      //TODO: API对接
      that.questions.splice(that.currentDeleteItem.index, 1);
      that.$message.success("删除成功！");
      console.log("删除");
    },
    editQuestion(row) {
      row.type = 3
      this.$router.push({ path: "/editQuestion", query: { question: row } })
    },
    getQuestionTotal() {
      //获取问题总数
      let that = this
      let url = this.$API.getService("Question", "getTotalNum")

      this.$API.get(url).then((res) => {
        console.log(res.data.data)
        that.totalQuestion = res.data.data
      })
    },
    getPage(page, pageNum) {
      let that = this
      let url = this.$API.getService("Question", "getPage")

      this.$API.post(url, {
        page: page,
        num: pageNum,
      }).then((res) => {
        console.log(res.data.data)
        that.questions = res.data.data
        that.QuestionsBack = that.questions
      })
    },
    resetEditFlag(flag) {
      for (var i = 0; i < this.cate.length; i++)
        this.cate[i].editFlag = flag
    },
    handleCurrentChange(val) {
      this.getPage(val, this.perPageQuestions)
    },
    handleReview(index, row) {
      this.questions[index].status = 0
    },
    handlePassed() {
      this.questions[index].status = 1
    },
    search(key) {
      if(!key)
        key = this.searchKey
      key = key.trim();
      let searchRes = [];
      for (var i = 0; i < this. QuestionsBack.length; i++) {
        if (this. QuestionsBack[i].question.indexOf(key) != -1)
          searchRes.push(this.QuestionsBack[i]);
      }
      this.questions = searchRes;
    },
    handleDelete(index, row) {
      row.index = index;
      this.currentDeleteItem = row;
    },
    handleSelectionChange(val) {
      this.multipleSelection = val;
    }
  },
  watch: {
    searchKey(key) {
      if (key == "") this.questions = this.QuestionsBack
      else this.search(key)
    }
  },
  mounted() {
    //获取问题数据
    this.getQuestionTotal()
    this.getPage(1, 1)
  },
  props: ["review"],
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
.article-title:hover {
  cursor: pointer;
  color: #20a0ff;
}
.article-title:visited {
  color: black;
}
</style>
