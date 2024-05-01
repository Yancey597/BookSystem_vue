<template>
  <div class="notice_container">
    <div class="items">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item>首页</el-breadcrumb-item>
        <el-breadcrumb-item>查看公告</el-breadcrumb-item>
      </el-breadcrumb>
    </div>

    <div class="board">
      <div class="lefts">
        <div class="bg-image">
          <div class="banner_header"><p>近期公告</p></div>
        </div>
      </div>
      <div class="rights">
        <div class="main"
             v-loading="loading"
             element-loading-text="拼命加载中"
             element-loading-spinner="el-icon-loading"
             element-loading-background="rgba(0, 0, 0, 0.8)">
          <div class="banner_main_item" v-for="item in lastThreeNotices" :key="item.noticeId">
            <div class="banner_main_item_header">
              <p>
                {{ item.noticeTitle }}
              </p>
            </div>
            <p style="margin-top:8px;font-size:14px;color: #c0c0c0">
              {{ item.createTime }}
            </p>
            <div class="banner_main">
              <p>{{ item.noticeContent }}</p>
            </div>

          </div>
        </div>
        <p style="color: #e2e2e2;font-size:1.5em;padding-top:1.5em;padding-left:32em">仅显示近期三条通知</p>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      text: "欢迎来到 AI Library, 敬请关注公告信息！",
      noticeList: [
        {
          noticeId: 0,
          noticeAdminId: Number,
          noticeTitle: "",
          noticeContent: "",
          createTime: "",
          updateTime: ""
        }
      ],
      loading: true
    };
  },
  computed: {
    lastThreeNotices() {
      return this.noticeList.slice(-3); // 返回最后三个元素
    }
  },
  methods: {
    async getRuleList() {
      this.loading = true;
      const {data: res} = await this.$http.get('user/get_noticelist')
      if (res.status !== 200) {
        this.loading = false;
        return this.$message.error(res.msg)
      }
      this.$message.success({
        message: res.msg,
        duration: 1000
      })
      this.noticeList = res.data;
      this.loading = false;
    }
  },
  mounted() {
    const containerWidth = this.$refs.scrollText.offsetWidth;
    const textWidth = this.$refs.scrollText.scrollWidth;

    // If the text is longer than the container, start the animation
    if (textWidth > containerWidth) {
      this.$refs.scrollText.style.animation = "scroll 10s linear infinite";
    }
  },
  created() {
    this.getRuleList();
  }
};
</script>

<style lang="less" scoped>
.bg-image {
  /* 当内容高度大于图片高度时，背景图像的位置相对于viewport固定 */
  //background-attachment: fixed;
  /* 让背景图基于容器大小伸缩 */
  /* 设置背景颜色，背景图加载过程中会显示背景色 */
  background-color: #464646;
  height: 100%;
  width: 100%;
  background-image: url(https://cdn.pixabay.com/photo/2018/07/11/16/53/book-3531412_1280.jpg);
  background-repeat: no-repeat;
}

.board {
  position: relative;
  width: 100%; /* 设置宽度为 300 像素 */
  height: 100%; /* 设置高度为 200 像素 */
  background-color: #fff;
  border-radius: 5px;
  box-shadow: 0 0 10px #ddd;
}

.notice_container {
  height: 90%;
  overflow: hidden;
}

.header {
  width: 100%;
  height: 50px;
  background-color: #222222;
  border-radius: 15px;
  // box-shadow: 0px 0px 5px 5px rgb(66, 142, 5);
  color: white;
  text-align: center;
  line-height: 50px;
  font-size: 24px;
}



@keyframes scroll {
  from {
    transform: translateX(100%);
  }
  to {
    transform: translateX(-100%);
  }
}

.banner {
  width: 100%;
  height: 100%;
  // background-color: pink;
  margin-top: 30px;
}

.lefts {
  float: left;
  width: 40%;
  height: 100%;
  background-color: rgb(0, 0, 0);
}

.rights {
  float: left;
  width: 60%;
  height: 100%;

}

.banner_header {
  width: 100%;
  height: 80px;
  // background-color: brown;

  p {
    color: white;
    font-size: 40px;
    text-align: left;
    line-height: 80px;
    padding-top: 30px;
    margin-left: 50px;
  }
}

.banner_main {
  //width: 100%;
  //height: 100%;
  //display: flex;
  //flex-direction: column;
  //align-items: center;
  //color: skyblue;
  padding-top: 10px;
  font-size: 18px;

  p {
    font-size: 20px;
  }
}

.banner_main_item:nth-child(n+2) {
  margin-top: 30px;
}

.banner_main_item:nth-child(n+2) {
  //background-color: #D1EEEE;
}

//.banner_main_item:nth-child(1) {
//  background-color: pink;
//}

.banner_main_item {
  width: 90%;
  height: 120px;
  padding-left: 40px;
  padding-top: 60px;

  .banner_main_item_header {
    width: 100%;
    height: 50px;
    font-weight: 1000;

    //border: 1px solid skyblue;
    box-sizing: border-box;

    p {
      //color: rgb(175, 129, 143);
      font-size: 2.5em;
      //text-align: center;
      line-height: 50px;
    }
  }

  .banner_main_item_main {
    width: 100%;
    height: 70px;
    background-color: white;
    //border: 1px solid skyblue;
    box-sizing: border-box;
    text-align: center;

    p {
      font-size: 20px !important;
      line-height: 70px;
    }
  }
}
</style>