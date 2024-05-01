<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
    <el-header>
      <div>
        <span>Welcome to Ai-Library</span>
        <span style="color: #ccc; font-size: 16px">图书管理员界面</span>
      </div>
      <el-menu :default-active="activePath" class="el-menu-horizontal" text-color="white"
               active-text-color="white" :router="true" mode="horizontal">
        <!--        <el-menu-item index="borrowbook" @click="saveNavState('borrowbook')">-->
        <!--          <i class="el-icon-collection"></i>-->
        <!--          <span class="nav-text"  slo="title">借阅图书</span>-->
        <!--        </el-menu-item>-->
        <el-menu-item index="returnbook" @click="saveNavState('returnbook')">
          <i class="el-icon-collection"></i>
          <span class="nav-text" slot="title">图书管理</span>
        </el-menu-item>
        <el-menu-item
            index="returnstatement"
            @click="saveNavState('returnstatement')"
        >
          <i class="el-icon-folder-checked"></i>
          <span class="nav-text" slot="title">借出报表</span>
        </el-menu-item>
        <el-menu-item
            index="borrowstatement"
            @click="saveNavState('borrowstatement')"
        >
          <i class="el-icon-folder-checked"></i>
          <span class="nav-text" slot="title">还书报表</span>
        </el-menu-item>

        <el-menu-item
            index="noticemanage"
            @click="saveNavState('noticemanage')"
        >
          <i class="el-icon-s-promotion"></i>
          <span class="nav-text" slot="title">发布公告</span>
        </el-menu-item>
      </el-menu>

      <div>
        <div>
          <el-avatar
              src="https://pic.imgdb.cn/item/661bf31c68eb9357135a9037.jpg"
              :size="35"
              style="margin-right: 10px"
          ></el-avatar>
        </div>
        <div class="user">管理员:{{ this.bookAdmin.bookAdminName }}</div>
        <el-button type="info" @click="logout">退出</el-button>
      </div>
    </el-header>
    <!-- 页面主体区域 -->
    <el-container>
      <!-- 侧边栏 -->
      <!-- 右侧内容主体 -->
      <el-main>
        <!-- 路由占位符 -->
        <router-view></router-view>
        <div class="footer">
          <p slot="reference"> 联系作者| ©2024-2025 By YanceyStudio，Special thanks to azhe<br/></p>
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
      isCollapse: false,
      //被激活的链接地址
      activePath: "",
      bookAdmin: {
        bookAdminId: Number,
        status: Number,
        username: "",
        password: "",
        bookAdminName: "",
        email: "",
        createTime: "",
        updateTime: ""
      }
    };
  },
  async created() {
    // this.getMenuList();
    this.activePath = window.sessionStorage.getItem("activePath");
    // console.log(this.activePath)
    // 先获取sessionStorage中的id
    const stringId = window.sessionStorage.getItem("bookAdminId");
    const id = parseInt(stringId);
    this.bookAdmin.bookAdminId = id;
    const {data: res} = await this.$http.post("bookadmin/getData", this.bookAdmin);
    // console.log(res);
    this.bookAdmin = res.data;
  },
  methods: {
    logout() {
      window.sessionStorage.clear();
      this.$router.push("/loginmanage");
    },

    //点击按钮，切换菜单的折叠与展开
    toggleCollapse() {
      this.isCollapse = !this.isCollapse;
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

.footer {
  position: fixed;
  bottom: 0px;
  left: 40%;
  color: #ccc;

  a {
    color: #ccc;
  }
}

.home-container {
  height: 100%;
}

.el-header {
  background-color: black;
  display: flex;
  justify-content: space-between;
  padding-left: 0px;
  align-items: center;
  color: #fff;
  font-size: 20px;

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

.iconfont {
  margin-right: 10px;
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

.user {
  margin-right: 15px;
  color: #ccc;
  font-size: 16px;
}

.el-menu-item {
  //width: 30px;
  font-size: 16px;
  text-align: justify;
}

.nav-text {
  width: 100px;
  color: #CCCCCC;
  //display: inline-block;
  padding-left: 20px;
  padding-right: 20px;
  text-align: justify;
}

// .el-menu-item{
//   color:rgb(135, 206, 235) !important;
// }
</style>
