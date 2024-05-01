<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
    <el-header>
      <div>
        <span>Welcome to Ai-Library</span>
        <span style="color: #ccc; font-size: 16px">借阅者界面</span>
      </div>
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
      <el-aside :width="isCollapse ? '64px' : '200px'">
        <div class="toggle-button" @click="toggleCollapse">|||</div>
        <!-- 侧边栏菜单区域 -->
        <el-menu :default-active="activePath" class="el-menu-vertical-demo" background-color="#fff" text-color="black"
                 active-text-color="#000" :router="true" :collapse="isCollapse" :collapse-transition="false">
          <el-menu-item index="index" @click="saveNavState('index')">
            <i class="iconfont icon-shouyefill"></i>
            <span slo="title">首页</span>
          </el-menu-item>
          <el-menu-item index="search" @click="saveNavState('search')">
            <i class="iconfont icon-sousuoxiao"></i>
            <span slot="title">图书查询</span>
          </el-menu-item>
          <!--          <el-menu-item index="rule" @click="saveNavState('rule')">-->
          <!--            <i class="iconfont icon-guizeshezhi"></i>-->
          <!--            <span slot="title">读者规则</span>-->
          <!--          </el-menu-item>-->
          <el-menu-item index="notice" @click="saveNavState('notice')">
            <i class="iconfont icon-gonggao1"></i>
            <span slot="title">查看公告</span>
          </el-menu-item>
          <el-menu-item index="information" @click="saveNavState('information')">
            <i class="iconfont icon-gerenxinxi"></i>
            <span slot="title">个人信息</span>
          </el-menu-item>
          <el-menu-item index="borrow" @click="saveNavState('borrow')">
            <i class="iconfont icon-tushuqikan"></i>
            <span slot="title">借阅信息</span>
          </el-menu-item>
          <el-menu-item index="violation" @click="saveNavState('violation')">
            <i class="iconfont icon-weizhangchaxun"></i>
            <span slot="title">违章信息</span>
          </el-menu-item>
          <el-menu-item index="comment" @click="saveNavState('comment')">
            <i class="iconfont icon-liuyan"></i>
            <span slot="title">读者留言</span>
          </el-menu-item>
          <el-menu-item index="intelligent" @click="saveNavState('intelligent')">
            <i class="el-icon-monitor"></i>
            <span slot="title">智能推荐</span>
          </el-menu-item>
          <!--          <el-menu-item index="chat" @click="saveNavState('chat')">-->
          <!--            <i class="el-icon-monitor"></i>-->
          <!--            <span slot="title">图书反馈</span>-->
          <!--          </el-menu-item>-->
        </el-menu>
      </el-aside>
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
              :typeSpeed="50"
              :backSpeed="50"
          >
            <span class="typing"></span>
          </vue-typed-js>
        </div>
        <el-main>
          <!-- 路由占位符 -->
          <!--        <router-view></router-view>-->
          <div class="footer">
            <!--          <el-popover  :width="150" trigger="hover">-->
            <el-popover  :width="150">

              <img
                  style="height: 100px; width: 100px"
              />
            </el-popover>
            <p> 联系作者| ©2024-2025 By YanceyStudio，Special thanks to azhe<br/></p>
            <a href="https://beian.miit.gov.cn">备案号:鄂ICP备2024050350号-2 |</a>
            <a href="https://beian.mps.gov.cn/#/query/webSearch">
              <img
                  style="height: 16px; width: 16px; margin: 5px 0px 0px 5px"
              />

            </a>
          </div>
        </el-main>
      </div>
    </el-container>
    <!--    <div class="types-text">-->
    <!--      123-->
    <!--    </div>>-->
  </el-container>

</template>

<script>
export default {
  name: "index",
  components: {
    swiperVue
  },
  created() {

  },

};
</script>
