<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
    <el-header>
      <div>
        <!-- <img src="../assets/heima.png" alt="" /> -->
        <span>Welcome to Ai-Library</span>
        <span style="color: #ccc; font-size: 16px">系统管理人员页面</span>
      </div>
      <el-menu :default-active="activePath" class="el-menu-horizontal" text-color="white"
               active-text-color="white" :router="true" mode="horizontal">
        <el-menu-item index="bookmanage" @click="saveNavState('bookmanage')">
          <span class="nav-text" slo="title">书籍管理</span>
        </el-menu-item>
        <el-menu-item index="booktype" @click="saveNavState('booktype')">
          <span class="nav-text" slot="title">书籍类型</span>
        </el-menu-item>
        <el-menu-item
            index="statementmanage"
            @click="saveNavState('statementmanage')"
        >
          <span class="nav-text" slot="title">借阅证管理</span>
        </el-menu-item>
        <el-menu-item
            index="statementsearch"
            @click="saveNavState('statementsearch')"
        >
          <span class="nav-text" slot="title">借阅信息查询</span>
        </el-menu-item>
        <el-menu-item
            index="statementrulemanage"
            @click="saveNavState('statementrulemanage')"
        >
          <span class="nav-text" slot="title">借阅规则管理</span>
        </el-menu-item>
        <el-menu-item
            index="bookadminmanage"
            @click="saveNavState('bookadminmanage')"
        >
          <span class="nav-text" slot="title">图书管理员管理</span>
        </el-menu-item>
        <el-menu-item
            index="adminmanage"
            @click="saveNavState('adminmanage')"
        >
          <span class="nav-text" slot="title">系统管理</span>
        </el-menu-item>
        <el-menu-item
            index="intelligent_analysis"
            @click="saveNavState('intelligent_analysis')"
        >
          <span class="nav-text" slot="title">智能分析</span>
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
        <div class="user">管理员:{{ this.admin.adminName }}</div>
        <el-button type="info" @click="logout">退出</el-button>
      </div>
    </el-header>
    <!-- 页面主体区域 -->
    <el-container>
      <!-- 侧边栏 -->

      <!-- 侧边栏菜单区域 -->

      <!-- 右侧内容主体 -->
      <el-main>
        <!-- 路由占位符 -->
        <router-view></router-view>
        <div class="footer">
          <el-popover  :width="150" trigger="hover">
            <p slot="reference"> 联系作者| ©2024-2025 By YanceyStudio，Special thanks to azhe<br/></p>
            <img
                style="height: 100px; width: 100px"
            />
          </el-popover>
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
      admin: {
        adminId: Number,
        status: Number,
        adminName: "",
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
    const stringId = window.sessionStorage.getItem("adminId");
    const id = parseInt(stringId);
    this.admin.adminId = id;
    const {data: res} = await this.$http.post("admin/getData", this.admin);
    this.admin = res.data;
  },
  methods: {
    logout() {
      window.sessionStorage.clear();
      this.$router.push("/loginadmin");
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

.el-aside {
  background-color: #fff;

  .el-menu {
    border-right: none;
  }
}

.el-main {
  background-color: #eaedf1;
  padding: 20px;
}

.iconfont {
  margin-right: 10px;
}


.user {
  margin-right: 15px;
  color: #ccc;
  font-size: 16px;
}

.el-menu-item:hover {
  background-color: #404040 !important;
}

// .el-menu-item{
//   color:rgb(135, 206, 235) !important;
// }
.el-menu-item {
  //width: 30px;
  font-size: 16px;
  text-align: justify;
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
  padding-left: 10px;
  padding-right: 10px;
  text-align: justify;
}
</style>
