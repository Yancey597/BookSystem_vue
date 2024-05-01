<template>
  <div class="bg-settings">
    <!-- 侧边栏 -->
    <!-- 右侧内容主体 -->
    <div class="type-text">
      <div class="texts">
        <div class="bg-img">

        </div>
        <vue-typed-js
            class="desc"
            :strings="['欢迎来到 AI Library!','Welcome to AI Library!']"
            :loop="true"
            :startDelay="300"
            :typeSpeed="100"
            :backSpeed="50"
        >
          <span class="typing"></span>
        </vue-typed-js>
      </div>
      <!-- 路由占位符 -->
      <!--        <router-view></router-view>-->
<!--      <div class="footer">-->
<!--        <p> 联系作者| ©2024-2025 By YanceyStudio，Special thanks to azhe<br/></p>-->
<!--        <a href="https://beian.miit.gov.cn">备案号:鄂ICP备2024050350号-2 |</a>-->
<!--        <a href="https://beian.mps.gov.cn/#/query/webSearch">-->
<!--          <img-->
<!--              style="height: 16px; width: 16px; margin: 5px 0px 0px 5px"-->
<!--          />-->
<!--          -->
<!--        </a>-->
<!--      </div>-->
    </div>
  </div>


</template>
<script>
export default {
  data() {
    return {
      //左侧菜单数据
      menulist: [],

      //是否折叠
      //被激活的链接地址
      activePath: "",
      user: {
        userId: Number,
        cardNumber: Number,
        ruleNumber: Number,
        status: Number,
        cardName: "",
        username: "",
        password: "",
        createTime: "",
        updateTime: "",
      },
    };
  },
  async created() {
    // this.getMenuList();
    this.activePath = window.sessionStorage.getItem("activePath");
    // console.log(this.activePath)
    // 先获取sessionStorage中的id
    const stringId = window.sessionStorage.getItem("userId");
    const id = parseInt(stringId);
    this.user.userId = id;
    const {data: res} = await this.$http.post("user/getData", this.user);
    console.log(res);
    window.sessionStorage.setItem('cardNumber', res.data.cardNumber)
    this.user = res.data;
  },
  async mounted() {
  },
  methods: {
    logout() {
      window.sessionStorage.clear();
      this.$router.push("/login");
    },

    //保存链接的激活状态
    saveNavState(activePath) {
      // console.log("first")
      window.sessionStorage.setItem("activePath", activePath);
      this.activePath = activePath;
      // console.log(this.activePath);
    },
  },
};
</script>
<style lang="less" scoped>
.desc {
  z-index: 5;
  font-size: 70px;
  color: white;
  display: inline-block;
  position: relative;
  left: 30%;
  top: -60%;
}

.texts {
  width: 100%;
  height: 100%;
  //position: relative;
}

.type-text {
  width: 100%;
  position: relative;
  height: 100%;
  //background-color: rgba(0, 0, 0,.5);
  //background-color: black;
}

//.type-text::before {
//  content: ""; /* 伪元素需要内容，即使它是空的 */
//  position: absolute; /* 绝对定位使得伪元素可以覆盖整个父元素 */
//  top: 0;
//  left: 0;
//  width: 100%;
//  height: 100%;
//  background-color: rgba(0, 0, 0, 0.4); /* 半透明的黑色遮罩 */
//  z-index: 2; /* 确保遮罩在内容之上 */
//}

.bg-img {
  position: relative; /* 确保图片在遮罩之上 */
  z-index: 1; /* 图片的z-index比遮罩高 */
  width: 100%;
  height: 100%;
  background-size: cover;

  background-image: url(https://pic.imgdb.cn/item/662a750b0ea9cb14032e9402.jpg);

}

.footer {
  position: fixed;
  bottom: 0px;
  left: 40%;
  color: white !important;
  z-index: 10;

  a {
    color: white !important;
  }
}

.home-container {
  height: 100%;

  //background-color: black;
}

.el-header {
  background-color: rgb(34, 34, 34);
  display: flex;
  justify-content: space-between;
  padding-left: 0px;
  align-items: center;
  color: #fff;
  font-size: 20px;
  border-radius: 0px;

  > div {
    display: flex;
    align-items: center;

    span {
      margin-left: 15px;
    }
  }
}


.el-main {
  background-color: #eaedf1;
  padding: 20px;
}

.user {
  margin-right: 15px;
  color: #ccc;
  font-size: 16px;
}

.el-menu-item:hover {
  background-color: #EAEDF1 !important;
}


.bg-settings {
  width: 110%;
  height: 110%;
  margin-left: -5em;
  margin-top: -2em;
}

// }</style>
