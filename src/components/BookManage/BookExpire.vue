<template>
  <div class="expire_container">
    <div class="board">
      <div class="lefts"></div>
      <div class="rights">
        <div style="position: absolute; left:46%; width: 20%;">
          <p style="margin-left:5%;margin-top:15%;height:50px;font-size:2em;">归还图书</p>
          <p style="margin-left:5%;font-size:1.2em;height:0px; color: #c1c1c1">
            <!--            <i class="el-icon-s-flag"></i> Books are the ever-burning lamps of accumulated wisdom.-->
            Return book
          </p>
        </div>

        <div class="book_number">
          <p>剩余天数: {{ this.expireInfo.expireDays }}</p>
          <p>图书编号: {{ this.expireInfo.bookNumber }}</p>
          <p>截止日期: {{ this.expireInfo.closeDate }}</p>
          <p>管理员编号: {{ this.expireInfo.bookAdminId }}</p>
          <span> 违章信息: </span>

          <el-input
              v-model="returnInfo.violationMessage"
              class="inputs"
          >
          </el-input>
          <div style="margin-top:10px">
            <span> 归还时间: </span>


            <el-date-picker
                class="inputs"
                v-model="returnInfo.returnDate"
                type="datetime"
                placeholder="选择归还日期时间"
                default-time="12:00:00"
                value-format="yyyy-MM-dd HH:mm:ss"
                prefix-icon="el-icon-date">


            </el-date-picker>
          </div>
          <div class="return_button">
            <el-button type="warning" @click="returnBook">归还</el-button>
          </div>
        </div>
        <div class="bookNumber">

        </div>
        <div class="closeDate">

        </div>

        <div class="violationMsg">

        </div>
        <div class="adminId">


        </div>
        <div class="returnDate">

        </div>

      </div>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      returnInfo: {
        returnDate: "",
        violationMessage: "",
        bookNumber: 0,
        violationAdminId: 0
      },
      expireInfo: {
        expireDays: 47,
        bookNumber: 192,
        closeDate: "",
        bookAdminId: 0
      }
    };
  },
  methods: {
    async getExpireInfo() {
      // 先获取路由传参中的图书编号
      const bookNumber = this.$route.query.bookNumber;
      // 发送axios请求
      const {data: res} = await this.$http.get('bookadmin/query_expire/' + bookNumber);
      // console.log(res);
      if (res.status !== 200) {
        return this.$message.error(res.msg)
      }
      this.$message.success(
          {
            message: res.msg,
            duration: 1000
          }
      )
      this.expireInfo = res.data;
      const id = window.sessionStorage.getItem('bookAdminId')
      this.expireInfo.bookAdminId = parseInt(id);
      this.returnInfo.bookNumber = this.expireInfo.bookNumber;
    },
    async returnBook() {
      // 对表单参数进行校验
      if (this.returnInfo.returnDate === "") {
        this.$message.error({
          message: "归还日期不能为空",
          duration: 1000
        })
        return;
      }
      this.returnInfo.violationAdminId = parseInt(window.sessionStorage.getItem('bookAdminId'))
      const {data: res} = await this.$http.post('bookadmin/return_book', this.returnInfo)
      // console.log(res);
      if (res.status !== 200) {
        return this.$message.error(res.msg)
      }
      this.$message.success(
          {
            message: res.msg,
            duration: 1000
          }
      )
      this.$router.push('/returnbook');
    }
  },
  created() {
    this.getExpireInfo();
  }
};
</script>

<style lang="less" scoped>
.return_button{
  margin-top:10px;
}
.book_number {
  margin-top: 15%;
  margin-left: 12%;

  p {
    font-size: 25px;
    //margin-top: 20px;
    margin-bottom: 20px;
  }

  span {
    font-size: 25px;
  }
}

.expire_container {
  height: 100%;
  position: relative;
}

.lefts {
  float: left;
  width: 40%;
  height: 100%;
  background-image: url(https://pic.imgdb.cn/item/662910ab0ea9cb1403850bb7.jpg);
  background-size: cover;
}

.rights {
  float: left;
  width: 60%;
  height: 100%;
}

.inputs {
  width: 30%;
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

.board {
  position: relative;
  width: 100%; /* 设置宽度为 300 像素 */
  height: 90%; /* 设置高度为 200 像素 */
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

.header {
  text-align: center;
  height: 100px;
  // background-color: pink;
  p {
    font-size: 30px;
    line-height: 100px;
  }
}

.banner {
  height: 680px;
  // background-color:brown;
  display: flex;
  flex-direction: column;
  align-items: center;

  .expireDays {
    margin-top: 20px;
    width: 700px;
    height: 100px;
    border: 2px solid #ccc;
    text-align: center;

    p {
      line-height: 100px;
      font-size: 24px;
    }
  }

  div:nth-child(n + 2) {
    margin-top: 25px;
  }
}
</style>