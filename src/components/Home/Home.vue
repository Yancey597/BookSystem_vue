<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
    <el-header>
      <div>
        <span>Welcome to Ai-Library</span>
        <span style="color: #ccc; font-size: 16px">借阅者界面</span>
      </div>

      <el-menu :default-active="activePath" class="el-menu-horizontal" text-color="white"
               active-text-color="white" :router="true" mode="horizontal">
        <el-menu-item index="index" @click="saveNavState('index')">
          <span class="nav-text">首页</span>
        </el-menu-item>
        <el-menu-item index="search" @click="saveNavState('search')">
          <span class="nav-text" slot="title">图书查询</span>
        </el-menu-item>
        <el-menu-item index="notice" @click="saveNavState('notice')">
          <span class="nav-text" slot="title">公告信息</span>
        </el-menu-item>
        <el-menu-item index="information" @click="saveNavState('information')">
          <span class="nav-text" slot="title">个人信息</span>
        </el-menu-item>
        <el-menu-item index="borrow" @click="saveNavState('borrow')">
          <span class="nav-text" slot="title">我的借阅</span>
        </el-menu-item>
        <el-menu-item index="violation" @click="saveNavState('violation')">
          <span class="nav-text" slot="title">查询违章</span>
        </el-menu-item>
        <el-menu-item index="comment" @click="saveNavState('comment')">
          <span class="nav-text" slot="title">留言天地</span>
        </el-menu-item>
        <el-menu-item index="intelligent" @click="saveNavState('intelligent')">
          <span class="nav-text" slot="title">智能推荐</span>
        </el-menu-item>
      </el-menu>

      <div>
        <div>
          <el-avatar src="https://pic.imgdb.cn/item/661bf31c68eb9357135a9037.jpg" :size="35"
                     style="margin-right: 10px"></el-avatar>
        </div>

        <div class="user">用户:{{ this.user.cardName }}</div>
        <el-button type="info" @click="logout">退出</el-button>
      </div>
    </el-header>
    <!-- 页面主体区域 -->
    <el-container>
      <!-- 侧边栏 -->

      <!--      <el-menu :width="isCollapse ? '64px' : '200px'">-->

      <!-- 右侧内容主体 -->
      <el-main>
        <!-- 路由占位符 -->
        <router-view></router-view>
        <div class="footer">
          <!--          <el-popover  :width="150" trigger="hover">-->

          <p> 联系作者| ©2024-2025 By YanceyStudio，Special thanks to azhe<br/></p>
          <a href="https://beian.miit.gov.cn">备案号:鄂ICP备2024050350号-2 |</a>
        </div>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>


export default {
  data() {
    return {
      //左侧菜单数据
      menulist: [],
      iconsObj: {
        125: "iconfont icon-user",
        103: "iconfont icon-tijikongjian",
        101: "iconfont icon-shangpin",
        102: "iconfont icon-danju",
        145: "iconfont icon-baobiao",
      },
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
    console.log(this.activePath)
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


    //点击按钮，切换菜单的折叠与展开
    //保存链接的激活状态
    saveNavState(activePath) {
      // console.log("first")
      window.sessionStorage.setItem("activePath", activePath);
      this.activePath = activePath;
      // console.log(this.activePath);
    },
    toGitee() {
      console.log(1123);
    }
  },
};
</script>

<style lang="less" scoped>
//.element::-webkit-scrollbar {
//  width: 0 !important
//}

.element {
  -ms-overflow-style: none;
}


.footer {
  position: fixed;
  bottom: 0px;
  left: 40%;
  color:#756155;
  z-index: 10;

  a {
    color: #756155
  }
}

.home-container {
  height: 100%;
  //background-color: black;
}

.el-header {
  background-color: black;
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


.el-menu {
  //color: red !important;
  background-color: black;
}

.el-menu-item {
  //width: 30px;
  font-size: 16px;
  text-align: justify;
}

.el-main {
  background-color: #eaedf1;
  overflow: hidden;
  padding: 20px;
  padding-left: 5em;
  padding-right: 5em;
}

//
//.iconfont {
//  margin-right: 10px;
//}


.user {
  margin-right: 15px;
  color: #ccc;
  font-size: 16px;
}

.el-menu-item:hover {
  background-color: #404040 !important;
  //border-bottom: 2px solid white !important;
}

.el-menu-horizontal {
  //color: red;
  //font-size: 20px;
  //text-align: center;
  background-color: black;
}

.nav-text {
  width: 100px;
  color: #CCCCCC;
  //display: inline-block;
  padding-left: 20px;
  padding-right: 20px;
  text-align: justify;
}
.element {
  -ms-overflow-style: none; /* IE and Edge */
}

.element::-webkit-scrollbar {
  display: none; /* Chrome, Safari, Opera */
}

// .el-menu-item{
//   color:rgb(135, 206, 235) !important;
// }</style>
