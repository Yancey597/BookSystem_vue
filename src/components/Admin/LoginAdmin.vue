<template>
  <div class="login_container">
    <!--    <div class="login_title">系统管理员登录界面</div>-->
    <div class="lefts">
      <!--          <div class="login_title">用户登录界面</div>-->
    </div>
    <div class="rights">
      <!--      <vue-particles-->
      <!--          class="login-bg"-->
      <!--          color="#757073"-->
      <!--          :particleOpacity="0.7"-->
      <!--          :particlesNumber="100"-->
      <!--          shapeType="circle"-->
      <!--          :particleSize="4"-->
      <!--          linesColor="#909090"-->
      <!--          :linesWidth="1"-->
      <!--          :lineLinked="true"-->
      <!--          :lineOpacity="0.4"-->
      <!--          :linesDistance="150"-->
      <!--          :moveSpeed="3"-->
      <!--          :hoverEffect="true"-->
      <!--          hoverMode="grab"-->
      <!--          :clickEffect="true"-->
      <!--          clickMode="push"-->
      <!--      >-->
      <!--      </vue-particles>-->
      <div class="login_box">
        <!-- 头像区域 -->
        <div class="avatar_box">
          <img src="https://pic.imgdb.cn/item/661bf31c68eb9357135a9037.jpg" alt=""/>
        </div>
        <div class="img-logo">
          <img src="https://pic.imgdb.cn/item/662d212c0ea9cb140386710b.png" width=80px>
        </div>
        <h2 style="margin-top:-5em; font-size:40px;margin-left: 0.5em">系统管理员登录</h2>
        <p style="color:#797979; margin-top:5px; font-size:19px;margin-left: 1em">log in to continue in  AI library Admin System</p>
        <!-- 登录表单区域 -->
        <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" label-width="0px" class="login_form">
          <!-- 用户名 -->
          <el-form-item prop="username" style="margin-left: -7em">
            <el-input v-model.trim="loginForm.username" prefix-icon="iconfont icon-gerenxinxi"></el-input>
          </el-form-item>
          <!-- 密码 -->
          <el-form-item prop="password" style="margin-left: -7em">
            <el-input v-model="loginForm.password" prefix-icon="iconfont icon-tianchongxing-" type="password"
                      @keyup.enter.native="login" :show-password="true" class=""></el-input>
          </el-form-item>
          <!-- 按钮区域 -->
          <div style="height:50px"></div>
          <el-form-item class="btns">
            <el-button type="primary" @click="login" :loading="loginLoading" style="height:50px;line-height: 12px;margin-left: -5.5em">
              登录
            </el-button>
            <el-button type="info" @click="resetLoginForm" style="height:50px;line-height: 12px;">重置</el-button>
          </el-form-item>
        </el-form>

      </div>
      <div class="switch">
        <div class="switch-btn" @click="goLogin">
          <i class="iconfont" style="font-size:34px;color:#797979">&#xe611;</i>
          <span style="font-weight:450;color:#797979">用户登录</span>
        </div>
        <div class="switch-btn" @click="goManage">
          <i class="iconfont" style="font-size:30px;color:#797979">&#xe6fb;</i>
          <span style="font-weight:450;color:#797979">图书管理员</span>
        </div>
        <div class="switch-btn" @click="goUser">
          <i class="iconfont" style="font-size:30px;color:#797979">&#xe612;</i>
          <span style="font-weight:450;color:#797979">系统管理员</span>
        </div>
      </div>
    </div>
    <div class="footer">
    </div>
    <div class="footer2">
      <p> By Yancey </p>
      <a href="https://beian.miit.gov.cn">| </a>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      //登录表单
      loginForm: {
        username: "yancey",
        password: "123456",
      },
      //登录表单规则的验证对象
      loginFormRules: {
        username: [
          {required: true, message: "用户名不能为空", trigger: "blur"},
          {
            min: 3,
            max: 20,
            message: "长度在 3 到 20 个字符",
            trigger: "blur",
          },
        ],
        password: [
          {required: true, message: "密码不能为空", trigger: "blur"},
          {
            min: 6,
            max: 15,
            message: "长度在 6 到 15 个字符",
            trigger: "blur",
          },
        ],
      },
      loginLoading: false
    };
  },
  methods: {
    resetLoginForm() {
      this.$refs.loginFormRef.resetFields();
    },
    login() {
      this.$refs.loginFormRef.validate(async (valid) => {
        // console.log(valid);
        //如果表单验证无效，直接返回
        if (!valid) {
          return;
        }
        this.loginLoading = true;
        const username = this.loginForm.username;
        const password = this.loginForm.password;
        //向数据库发送axios请求，如果登录成功，就跳转
        const {data: res} = await this.$http.post(
            "admin/login",
            {
              username,
              password
            }
        );
        if (res.status !== 200) {
          this.loginLoading = false;
          return this.$message.error(res.msg);
        }
        this.$message.success("登录成功");
        this.loginLoading = false;
        // console.log(res);
        window.sessionStorage.setItem("token", res.map.token);
        window.sessionStorage.setItem("adminId", res.map.id);
        this.$router.push("/homeadmin");
      });
    },
    goLogin() {
      if (this.$route.path !== "/login") {
        this.$router.push("/login");
      }
    },
    goUser() {
      if (this.$route.path !== "/loginadmin") {
        this.$router.push("/loginadmin");
      }
    },
    goManage() {
      if (this.$route.path !== "/loginmanage") {
        this.$router.push("/loginmanage");
      }
    },
  },
}
</script>

<style lang="less" scoped>
.img-logo {
  position: absolute;
  left:90%;
  top:-80%;
}
.el-input {
  width: 400px;
}

/* 调整输入框的高度 */
/* 使用更具体的选择器 */
.el-form-item > .el-input__inner {
  height: 200px !important;
  border-radius: 10px;
  color: #333;
}


.el-input__icon {
  font-size: 20px;
}

/* 设置输入框中光标的颜色 */


/* 设置输入框的边框聚焦时的颜色 */
.el-input__inner:focus {
  border-color: #0b2e13 !important;
}

.login-bg {
  position: absolute; /* 将粒子效果定位在页面上 */
  top: 0;
  left: 0;
  width: 100%; /* 设置宽度为页面宽度 */
  height: 100%; /* 设置高度为页面高度 */
  z-index: 1; /* 将粒子效果置于页面底层，避免遮挡其他内容 */
  pointer-events: none; /* 禁止粒子效果响应鼠标事件，避免影响页面交互 */
  //background-color: #000; /* 设置背景颜色，以备粒子效果不可见时使用 */
  /* 可以根据需要添加更多属性 */
}

.lefts {
  float: left;
  width: 60%;
  height: 100%;
  background: url(https://pic.imgdb.cn/item/661bee5368eb9357135748fe.jpg) no-repeat 0px 0px;
  background-size: cover;
}

.rights {
  float: right;
  width: 40%;
  height: 100%;
  position: relative;
}

.switch {
  width: 50%;
  position: absolute;
  top: 80%;
  left: 20%;
  //right: 50%;

  //padding: 10px; /* 添加内边距以容纳盒子 */
  box-sizing: border-box; /* 确保宽度包括内边距 */
  display: flex; /* 使用Flexbox进行布局 */
  justify-content: center; /* 水平居中 */
  align-items: center; /* 垂直居中（如果需要的话） */
  justify-content: space-around;
}

.switch-btns {
  position: relative;
  height: 100px;
  width: 40px;
  margin-right: 20px;
  /* 其他样式 */
  display: flex;
  align-items: center; /* 垂直居中（如果需要的话） */
  justify-content: center;
  //margin: 0 20px;
}

.switch-btns i {
  //display: inline-block;
  //background-color: #000;
}

.input-style {
  border-radius: 20px; /* 设置边框圆角 */
  border: 1px solid #ccc; /* 设置边框颜色 */
  padding: 10px 15px; /* 设置内边距 */
  font-size: 14px; /* 设置字体大小 */
  color: #333; /* 设置字体颜色 */
  box-shadow: none; /* 取消阴影 */
}


.footer2 {
  position: absolute;
  bottom: 0px;
  left: 70%;
  color: #ccc;

  a {
    color: #ccc;
  }
}

.login_container {
  // background-color: #2b4b6b;
  //background: url(https://pic.imgdb.cn/item/661bee5368eb9357135748fe.jpg) no-repeat 0px 0px;
  //background-size: cover;
  height: 100%;
  width: 100%;
  background-color: #fff;
  overflow: auto;
}

.login_box {
  height: 300px;
  width: 450px;
  //background-color: rgba(0, 0, 0, .1);
  border-radius: 14px;
  position: absolute;
  top: 48%;
  left: 60%;
  transform: translate(-50%, -50%);

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
}

.login_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  text-align: left; /* 左对齐 */
  box-sizing: border-box;
}

.btns {
  //display: flex;
  //justify-content: left;
}

.login_form .el-button {
  text-align: left;
}

.login_title {
  position: relative;
  top: 5%;
  font-size: 36px;
  color: white;
  text-align: center;
  font-weight: 700;
  //控制字体间距
  letter-spacing: 10px;
}

.footer {
  display: flex;
  position: absolute;
  flex-direction: column;
  bottom: 0;
  right: 0;
  width: 100px;
  height: 120px;
  color: pink;

  span {
    // width: 100%;
    // background-color: red;
    text-align: center;
  }
}
</style>
