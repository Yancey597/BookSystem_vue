<template>
  <div class="information_container">
    <div class="ite ms">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item>首页</el-breadcrumb-item>
        <el-breadcrumb-item>个人信息</el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <!--    <div class="backgroundImg">-->
    <!--      <img-->
    <!--          alt="背景图片"-->
    <!--      />-->
    <!--    </div>-->
    <div class="board">
      <div class="lefts">

        <div class="avatar_box">
          <img src="https://pic.imgdb.cn/item/661bf31c68eb9357135a9037.jpg" alt=""/>
        </div>
        <div style="position: absolute; left:10%; width: 20%;">
          <p style="margin-left:4%;margin-top:25%;height:50px;font-size:2em;">个人信息</p>
          <p style="margin-left:5%;font-size:1.2em;height:0px; color: #c1c1c1">
            <!--            <i class="el-icon-s-flag"></i> Books are the ever-burning lamps of accumulated wisdom.-->
            Information
          </p>

        </div>

        <div class="information_banner"
             v-loading="loading"
             element-loading-text="拼命加载中"
             element-loading-spinner="el-icon-loading"
             element-loading-background="rgba(0, 0, 0, 0.8)">
          <div class="information_banner_left">
            <div class="banner_left_main" v-if="show">
              <div class="number" style="padding:0px 0px !important">
                <i class="el-icon-collection-tag"></i> 借阅证编号:
                {{ this.user.cardNumber }}
              </div>
              <div class="name">
                <i class="iconfont icon-gerenxinxi"></i> 借阅证姓名:
                {{ this.user.cardName }}
              </div>
              <div class="rule">
                <i class="iconfont icon-guizeshezhi"></i> 规则编号:
                {{ this.user.ruleNumber }}
              </div>
              <div class="status">
                <i class="el-icon-refresh"></i> 状态:
                {{ this.user.status === 1 ? "可用" : "禁用" }}
              </div>

              <el-button type="primary" class="changePWD" @click="showEditDialog" v-if="show"
              >修改密码
              </el-button
              >
              <el-button type="primary" class="changeINF" @click="showEditDialog" v-if="show"
              >修改信息
              </el-button>
            </div>
          </div>

        </div>


        <el-dialog
            title="修改密码"
            :visible.sync="editDialogVisible"
            width="50%"
            @close="editDialogClosed"
        >
          <el-form
              :model="editForm"
              ref="editFormRef"
              :rules="editFormRules"
              label-width="120px"
          >
            <el-form-item label="新密码" prop="password">
              <el-input v-model="editForm.password" type="password" placeholder="请输入新密码"></el-input>
            </el-form-item>
            <el-form-item label="新密码" prop="confirmPassword">
              <el-input v-model="editForm.confirmPassword" type="password" placeholder="请再次输入新密码"></el-input>
            </el-form-item>
          </el-form>
          <span slot="footer" class="dialog-footer">
          <el-button @click="editDialogVisible = false">取 消</el-button>
          <el-button type="primary" @click="changePassword"
          >确 定</el-button
          >
        </span>
        </el-dialog>
      </div>
      <div class="rights">

      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    var validatePass2 = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请再次输入密码'));
      } else if (value !== this.editForm.password) {
        callback(new Error('两次输入密码不一致!'));
      } else {
        callback();
      }
    };
    return {
      user: {
        ruleNumber: Number,
        cardNumber: Number,
        status: Number,
        userId: Number,
        cardName: "",
        username: "",
        password: "",
        createTime: "",
        updateTime: "",
      },
      editForm: {
        password: "",
        confirmPassword: "",
      },
      editFormRules: {
        password: [
          {required: true, message: "请输入新密码", trigger: "blur"},
          {min: 6, max: 15, message: "新密码长度在6-15个字符", trigger: "blur"}
        ],
        confirmPassword: [
          {validator: validatePass2, trigger: "blur"}
        ]

      },
      editDialogVisible: false,
      show: false,
      loading: true
    };
  },
  methods: {
    //让修改公告的对话框可见,并从数据库中回显数据
    showEditDialog() {
      // 让修改公告的对话框可见
      this.editDialogVisible = true;
    },
    //监听修改对话框的关闭，一旦对话框关闭，就重置表单，回显数据
    editDialogClosed() {
      this.$refs.editFormRef.resetFields();
    },
    async getUserInformaton() {
      // 先从sessionStorage中获取用户id
      const userId = window.sessionStorage.getItem("userId");
      // 发送axios请求，携带用户id，获取个人信息
      this.loading = true;
      const {data: res} = await this.$http.get(
          "user/get_information/" + userId
      );
      if (res.status !== 200) {

        return this.$message.error(res.msg);
      }
      this.$message.success({
        message: res.msg,
        duration: 1000,
      });
      this.user = res.data;
      this.show = true;
      this.loading = false;

    },
    async changePassword() {

      const {data: res} = await this.$http.post('user/update_password', {
        password: this.editForm.password,
        userId: window.sessionStorage.getItem('userId')
      })
      if (res.status !== 200) {
        return this.$message.error(res.msg);
      }
      this.$message.success(res.msg)
      this.editDialogVisible = false;
      this.$refs.editFormRef.resetFields();
      window.sessionStorage.clear();
      this.$router.push("/login");
    }
  },
  created() {
    this.getUserInformaton();
  },
};
</script>

<style lang="less" scoped>
.lefts {
  float:left;
  width: 40%;
  height: 100%;
}
.rights{
  float:left;
  width:60%;
  height:100%;
  background-image:url(https://pic.imgdb.cn/item/662910ab0ea9cb1403850bb7.jpg);
  background-size:cover;
}

.avatar_box {
  width: 73px;
  height: 73px;
  border: 1px solid #eee;
  border-radius: 50%;
  padding: 10px;
  box-shadow: 0 0 10px #ddd;
  position: absolute;
  top: 10%;
  left: 4%;
  //transform: translate(-50%, -50%);
  background-color: #fff;

  img {
    //margin-top: 50px;
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #eee;
  }
}

.information_container {
  position: relative;
  height: 100%;
}

.board {
  position: relative;
  width: 100%; /* 设置宽度为 300 像素 */
  height: 85%; /* 设置高度为 200 像素 */
  background-color: #fff;
  border-radius: 5px;
  box-shadow: 0 0 10px #ddd;
}

.backgroundImg {
  position: absolute;
  width: 100%;
  height: 100%;

  img {
    width: 100%;
    height: 100%;
    opacity: 0.3;
  }
}

.information_header {
  height: 200px;
  // background-color: pink;
  //text-align: center;

  p:nth-child(1) {
    line-height: 140px;
    color: black;
    font-size: 36px;
  }

  p:nth-child(2) {
    color: black;
    font-size: 24px;
  }
}

.information_banner {
  display: flex;
  flex-direction: row;
  height: 400px;
  weight:100%;
  //margin-bottom: 100px;
  .information_banner_left {
    flex: 0.5;
    // background-color: brown;
    text-align: left;
    margin-top: 5em;
    margin-left: 10em;
  }

  .information_banner_right {
    flex: 0.5;
    // background-color: skyblue;
    text-align: left;
    line-height: 400px;
  }
}

.banner_left_main {
  margin-top: 120px;
  color: black;
  font-size: 1.4em;
  font-align: left;
  width:1000px;
  div {
    margin-top: 15px;
  }
}

.changePWD {
  position: absolute;
  height: 7% !important;
  font-size: 20px;
  width: 8%;
  line-height: 5px;
  top: 70%;
  left: 10%;
}

.changeINF {
  position: absolute;
  height: 7% !important;
  font-size: 20px;
  width: 8%;
  line-height: 5px;
  top: 70%;
  left: 18%;
  background-color: #c0c0c0;
}
</style>
