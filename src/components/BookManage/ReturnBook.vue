<template>
  <div class="borrowbook_container">
    <!--    <div class="header"><p>查看图书是否逾期</p></div>-->
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
          <el-input placeholder="请输入图书编号" v-model="bookNumber" class="inputs">
            <el-button slot="prepend" icon="el-icon-collection"></el-button
            >
          </el-input>
          <div style="margin-top: 10px">
            <el-button type="warning" class="query_button" @click="goExpire">查询</el-button>
          </div>


        </div>

        <div style="position: absolute; left:46%; width: 20%;">
          <p style="margin-left:4%;margin-top:15%;height:50px;font-size:2em;">借阅图书</p>
          <p style="margin-left:5%;font-size:1.2em;height:0px; color: #c1c1c1">
            <!--            <i class="el-icon-s-flag"></i> Books are the ever-burning lamps of accumulated wisdom.-->
            Borrow book
          </p>
          <!--          <el-input placeholder="请输入借阅证号" v-model.number="borrowInfo.cardNumber">-->
          <!--            <el-button slot="prepend" icon="el-icon-notebook-2"></el-button>-->
          <!--          </el-input>-->
        </div>
        <div class="book_number">
          <el-input placeholder="请输入借阅证号" class="inputs" v-model.number="borrowInfo.cardNumber">
            <el-button slot="prepend" icon="el-icon-notebook-2"></el-button>
          </el-input>
          <div style="margin-top:10px;">
            <el-input placeholder="请输入图书编号" class="inputs" v-model.number="borrowInfo.bookNumber">
              <el-button slot="prepend" icon="el-icon-collection"></el-button
              >
            </el-input>
          </div>
          <div style="margin-top: 10px">
            <el-date-picker
                v-model="borrowInfo.borrowDate"
                type="datetime"
                placeholder="选择日期时间"
                default-time="12:00:00"
                value-format="yyyy-MM-dd HH:mm:ss"
                class="timesel"
            ></el-date-picker>
          </div>
          <div style="margin-top: 10px">
            <el-button type="primary" @click="borrowBook">借阅</el-button>

          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      bookNumber: "",
      borrowInfo: {
        cardNumber: "",
        bookNumber: "",
        borrowDate: "",
        bookAdminId: 0
      },
    };
  },
  methods: {
    async goExpire() {
      // 发送axios请求，携带图书编号  如果返回状态码为成功 跳转逾期的页面 否则提示未借出图书
      const {data: res} = await this.$http.get(
          "bookadmin/query_book/" + this.bookNumber
      );
      if (res.status !== 200) {
        return this.$message.error(
            {
              message: res.msg,
              duration: 1000
            }
        );
      }
      // console.log(res);
      // this.$message.success(res.msg);
      this.$router.push({
        name: "bookexpire",
        query: {
          bookNumber: this.bookNumber
        }
      });
    },
    async borrowBook(){
      // console.log(this.borrowInfo.borrowDate);
      if(this.borrowInfo.borrowDate=== ""){
        return this.$message.error({
          message:"借阅时间不能为空",
          duration:1000
        })
      }
      this.borrowInfo.bookAdminId = parseInt(window.sessionStorage.getItem('bookAdminId'))
      const {data:res} = await this.$http.post('bookadmin/borrow_book',this.borrowInfo)
      if(res.status !== 200){
        return this.$message.error(res.msg)
      }
      this.$message.success(res.msg)
      // 清空数据
      this.borrowInfo.bookNumber = "";
      this.borrowInfo.cardNumber = "";
      this.borrowInfo.borrowDate = "";
    }
  },
};
</script>

<style lang="less" scoped>
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

.borrowbook_container {
  position: relative;
  height: 100%;
}

//.borrowbook_container {
//  display: flex;
//  flex-direction: column;
//  align-items: center;
//
//  .header {
//    width: 100%;
//    height: 100px;
//    // background-color: pink;
//    p {
//      color: black;
//      font-size: 30px;
//      text-align: center;
//      line-height: 60px;
//    }
//  }
//
//  .banner {
//    display: flex;
//    flex-direction: column;
//    align-items: center;
//    width: 60%;
//    height: 400px;
//    // background-color: brown;
//    div {
//      margin-top: 15px;
//      // margin-left: ;
//    }
//
//    .borrow_date {
//      margin-left: 8px;
//    }
//
//    .query_button {
//      margin-top: 30px;
//    }
//  }
//}
.book_number {
  margin-top: 15%;
  margin-left: 12%;
}

.inputs {
  width: 30%;
}

.el-button {

  line-height: 20px;
}

.query_button {
  //margin-left: 10px;
  //height: 10px;
}

.timesel {
  width: 30%;
}
</style>