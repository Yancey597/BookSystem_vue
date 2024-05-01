<template>
  <div class="chat-container">
    <div class="board">
      <h2 style="text-align: center;color:black;font-size:2em; padding-top:20px">AI 慧读推荐</h2>
      <!--      <el-card class="chat" style="margin-top:10px;height: 80vh;border-radius: 15px;border-color: #ccc;">-->
      <el-card class="chat" style="margin-top:10px;height: 77vh;">
        <el-card class="main_chat">
<!--        <el-card class="main_chat" ref="mainChat">-->
          <div v-for="item in messages" :key="message.id" class="message">
            <div v-if="item.inputMessage!=null&&item.inputMessage!==''" class="user-message">
              {{ item.inputMessage }}
            </div>
            <div v-if="item.aiResult!=null&&item.aiResult!==''" class="bot-message">{{ item.aiResult }}</div>
          </div>
        </el-card>
        <el-input
            placeholder="请输入 Prompt，eg：我想理解微积分相关的知识，请帮我推荐书籍。"
            v-model.trim="inputMessage"
            :disabled="loading"
            @keyup.enter.native="sendMessage"
        >
          <el-button
              slot="append"
              icon="el-icon-s-promotion"
              v-loading="loading"
              @click="sendMessage"
              :disabled="loading"
          ></el-button>
        </el-input
        >
      </el-card>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      messages: [],
      inputMessage: "",
      message: {
        inputMessage: "",
        aiResult: "",
        userId: null
      },
      loading: false
    }
  },
  methods: {
    /**
     * 发送消息，接受AI结果
     * @returns {Promise<ElMessageComponent>}
     */
    async sendMessage() {
      // 将用户输入的消息添加到消息列表中
      if (this.inputMessage.trim() === "") {
        return this.$message.info({
          message: "文本不能为空",
          duration: 1500
        })
      }
      // 先判断文本内容是否为空或者是无意义的数字，如果是提示用户信息
      // /^[a-zA-Z0-9\u4e00-\u9fa5]+$/.test(this.inputMessage)
      if (this.inputMessage.trim()===''||/^\d+$/.test(this.inputMessage)||/^[a-zA-Z]+$/.test(this.inputMessage)) {
        this.$message.info({
          message: "请不要输入无意义的内容",
          duration: 1000
        });
        return;
      }
      if (this.inputMessage.trim() !== "") {
        var userId = window.sessionStorage.getItem("userId");
        this.messages.push({
          id: Date.now(),
          inputMessage: this.inputMessage,
          userId: parseInt(userId)
        });
        // 这里可以是调用 GPT API 的逻辑
        this.message.inputMessage = this.inputMessage.trim();
        this.message.userId = userId;
        console.log(this.userId);
        // 清空输入框
        this.inputMessage = "";
        this.loading = true;
        this.$message.info({
          message: "文本输入越长，接口调用时间越长，请耐心等待10秒左右",
          duration: 2500
        })
        const {data: res} = await this.$http.post("user/ai_intelligent", this.message)

        if (res.status !== 200) {
          this.loading = false;
          return this.$message.error(res.msg);
        }
        this.$message.success({
          message: res.msg,
          duration: 1500
        })
        this.messages.push({
          id: Date.now(),
          aiResult: res.data,
          userId: userId
        });
        this.loading = false;

      }
    },
    /**
     * 获取最近五条该用户和AI交流的信息
     * @returns {Promise<void>}
     */
    async getHistoryRecords() {
      var userId = window.sessionStorage.getItem("userId");
      const {data: res} = await this.$http.get("user/ai_list_information/" + userId)
      if (res.status !== 200) {
        return this.$message.error(res.msg);
      }
      this.$message.success({
        message: res.msg,
        duration: 1500
      })
      res.data.forEach((item) => {
        this.messages.push(item);
      })
    }
  },
  created() {
    this.getHistoryRecords();
  }
}
</script>

<style scoped lang='less'>

.board {
  position: relative;
  width: 100%; /* 设置宽度为 300 像素 */
  height: 100%; /* 设置高度为 200 像素 */
  background-color: #fff;
  border:none !important;
}

//.chat-container {
//  display: flex;
//  flex-direction: column;
//  height: 100vh;
//  border:none !important;
//}
.main_chat {
  //border:none !important;
  //border-radius: 5px;
  //box-shadow: 0 0 10px #ddd;
  box-shadow: none !important;
  margin-bottom: 10px;
  height: 60vh;
  width: 100%;
  background-size: cover;
  overflow-y: scroll;
}

.message {
  display: flex;
  flex-direction: column;
}

.user-message {
  display: flex;
  flex-direction: row;
  background-color: #222222;
  padding: 10px;
  border-radius: 10px;
  align-self: flex-end;
  text-align: right;
  margin-top: 10px;
  color: white;

}

.bot-message {
  background-color: #f4f6f8;
  padding: 10px;
  border-radius: 10px;
  text-align: left;
  align-self: flex-start;
  margin-top: 10px;
  white-space: pre-wrap;
}
</style>
