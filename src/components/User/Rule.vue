<template>


  <div class="rule_container">
    <div class="items">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item>首页</el-breadcrumb-item>
        <el-breadcrumb-item>个人信息</el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="board">
      <div class="header">
        <p style="text-align: center;">个人信息</p>
      </div>

      <div class="banner">
        <div class="avatar_box">
          <img src="https://pic.imgdb.cn/item/661bf31c68eb9357135a9037.jpg" alt=""/>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
import {Loading} from 'element-ui';

export default {
  data() {
    return {
      ruleList: [
        {
          ruleId: 0,
          bookRuleId: Number,
          bookDays: Number,
          bookLimitNumber: Number,
          bookOverdueFee: Number,
          bookLimitLibrary: "",
          createTime: "",
          updateTime: "",

        }
      ],
      loading: true
    }
  },
  methods: {
    async getRuleList() {
      const {data: res} = await this.$http.get("user/get_rulelist")
      if (res.status !== 200) {
        this.loading = false;
        return this.$message.error(res.msg);
      }
      this.$message.success({
        message: res.msg,
        duration: 1000
      })
      this.loading = false;
      this.ruleList = res.data
    }
  },
  created() {
    this.getRuleList()
  }
};
</script>

<style lang="less" scoped>
.avatar_box {
  width: 73px;
  height: 73px;
  border: 1px solid #eee;
  border-radius: 50%;
  padding: 10px;
  box-shadow: 0 0 10px #ddd;
  position: relative;
  left: -15%;
  top: -20% ;
  transform: translate(-50%, -50%);
  background-color: #fff;

  img {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #eee;
  }
}
.avatar_box {
  width: 93px;
  //height: 100%;
  border: 1px solid #eee;
  border-radius: 50%;
  padding: 10px;
  box-shadow: 0 0 10px #ddd;
  position: absolute;
  left: 100%;
  margin-top: 40px;
  margin-left: 40px;

  //transform: translate(-50%, -50%);
  background-color: #fff;

  img {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #eee;
  }
}
.board{
  position: relative;
  width: 95%; /* 设置宽度为 300 像素 */
  height: 95%; /* 设置高度为 200 像素 */
  background-color: #fff;
  border-radius:5px;
  box-shadow: 0 0 10px #ddd;
}
.rule_container {
  position: relative;
  width: 95%; /* 设置宽度为 300 像素 */
  height: 95%; /* 设置高度为 200 像素 */
}

.header {
  position: absolute;
  left: 50%;
  top:3%;
  transform: translate(-50%);

  p {
    font-size: 36px;

  }
}

.items {

}

.banner {
  display: flex;
  flex-direction: column;
  position: absolute;
  //left: 50%;
  transform: translate(-50%);
}

.el-tooltip {
  width: 500px;
  font-size: 16px;
  color: white;
}

.el-tooltip:hover {
  color: black;
}

.el-tooltip:nth-child(1) {
  margin-top: 100px;
}

.el-tooltip:nth-child(n+2) {
  margin-top: 50px;
  margin-left: 0px;
}

.el-tooltip:nth-child(odd) {
  background-color: pink;
}

.el-tooltip:nth-child(even) {
  background-color: rgb(49, 176, 213)
}

.content_tip {
  font-size: 16px;
}
</style>