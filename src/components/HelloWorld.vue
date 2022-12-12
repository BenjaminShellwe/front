<template>
  <div>
    <el-row>
      <el-col :span="6" style="margin-right: 5px; max-width: 470px;">
        <h1>{{ msg }}</h1>
        Spring Boot(2.7[Java11+]) Vue.js<br>
        日志查看工具(内部工具)<br>
        遇到问题与shellwe反馈<br>
        目前你只能找我本人反馈
        <el-divider />
        <div>
          <span style="font-size: 22px; ">简易控制台：</span>
          <div class="main" ref="main">
            <div id="simpleConsole" ref="content">
              <li v-for="item in message" :key="item" style="list-style:none;">{{ item }}</li>
            </div>
          </div>
        </div>
        <el-card shadow="hover">
          <template #header>
            <div class="card-header">
              <h2 style="margin: 0">操作板块</h2>
            </div>
          </template>
          <el-form
              :model="form"
              label-width="90px"
          >
            <h3>基础查询</h3>
            <el-form-item label="MsgId值">
              <el-input v-model="form.msgid" placeholder="输入MsgId值查询" clearable />
            </el-form-item>
            <el-form-item label="TraceNo值">
              <el-input v-model="form.traceno" placeholder="输入TraceNo值查询" clearable />
            </el-form-item>
            <el-collapse>
              <el-collapse-item title="高级查询">
                以下功能后期设计<br>
                正则表达式<br>
                自定义范围<br>
                调整模糊度<br>
                调整时间范围<br>
                直接选择日志文件<br>
              </el-collapse-item>
            </el-collapse>
            <el-form-item style="margin-top: 15px;">
              <el-button type="primary" @click="getWebSocket">查询</el-button>
              <el-button>清空所有</el-button>
            </el-form-item>
          </el-form>
        </el-card>

      </el-col>
      <el-col :span="17">
        <el-card shadow="hover">
          <template #header>
            <div class="card-header">
              <el-row>
                <el-col :span="2">
                  <h2 style="margin: 0">日志内容</h2>
                </el-col>
                <el-col :span="22">
                  <el-button @click="getWebSocket">清空日志</el-button>
                </el-col>
              </el-row>
            </div>
          </template>
          <el-scrollbar ref="scrollbar" height="60px">

          </el-scrollbar>

        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import {reactive} from "vue";
import { ref } from "vue"
const num = ref(5)
let time, items
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      form,
      getWebSocket,
      message: [],
      num,
      time,
      items
    }
  },
  mounted() {
    // WebSocket
    if ('WebSocket' in window) {
      this.websocket = new WebSocket('ws://192.168.40.254/channel/log')
      // alert('连接浏览器')
      this.initWebSocket()
    } else {
      alert('当前浏览器 不支持')
    }


  },
  updated() {

  },
  beforeUpdate(){

  },
  watch() {

  },
  methods: {
    initWebSocket () {
      // 连接错误
      this.websocket.onerror = this.setErrorMessage

      // 连接成功
      this.websocket.onopen = this.setOnopenMessage

      // 收到消息的回调
      this.websocket.onmessage = this.setOnmessageMessage

      // 连接关闭的回调
      this.websocket.onclose = this.setOncloseMessage

      // 监听窗口关闭事件，当窗口关闭时，主动去关闭websocket连接，防止连接还没断开就关闭窗口，server端会抛异常。
      window.onbeforeunload = this.onbeforeunload



    },
    setErrorMessage () {
      console.log('WebSocket连接发生错误 状态码：' + this.websocket.readyState)
    },
    setOnopenMessage () {
      console.log('WebSocket连接成功 状态码：' + this.websocket.readyState)
    },
    setOnmessageMessage (event) {
      // 根据服务器推送的消息做自己的业务处理
      this.message.push(event.data)
      // 判断当前DOM生成状态
      let dom = document.getElementById('simpleConsole')
      if (dom) {
        this.$nextTick(() => {
          this.$refs.main.scrollTop = this.$refs.content.scrollHeight;
        })
      }
    },
    setOncloseMessage () {
      console.log('WebSocket连接关闭 状态码：' + this.websocket.readyState)
    },
    onbeforeunload () {
      this.closeWebSocket()
    },
    closeWebSocket () {
      this.websocket.close()
    },
    // 返回log列表
    sortMessageList (m) {
      let message = []
      message.length = 10
      // 顺序遍历写入
      for (let i = 0; i < 10; i ++) {
        // 找到为空的下标写入
        if (message[i] == null) {
          message[i] = m
        } else {
          //   数组已满删除下标为最后一位的数据
          message[9] = null
          //   剩余原数组数据向后移一位
          for (let j = 8;j > 0; j --){
            message[j] = message[j + 1]
          }
        }
      }
      // 返回列表
      console.log(message)
      // 后端传过来的含ANSI编码的数据需要转义
      return message
    },
  }
}


const form = reactive({
  msgid: '',
  traceno: '',
})


const getWebSocket = () => {
  fetch('http://192.168.40.254:80/log ', {
    method: 'GET',
  }).then(response => {
    if (response.ok){
      // eslint-disable-next-line no-unused-vars
      response.json(message => {
        // ignore

        // document.getElementById("log-container").innerHTML = event.data
      })
    }
  })

}


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.main{
  width: 100%;
  height: 200px;
  padding: 10px;
  margin-bottom: 10px;
  box-sizing: border-box;
  background-color: pink;
  overflow: auto;
  overflow-x: hidden;
}
/*滚动条样式 innerbox为需要滚动的区域盒子*/
.main::-webkit-scrollbar {/*滚动条整体样式*/
  width: 4px;     /*高宽分别对应横竖滚动条的尺寸*/
  height: 4px;
}
.main::-webkit-scrollbar-thumb {/*滚动条里面小方块*/
  border-radius: 5px;
  -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
  background: rgba(0,0,0,0.2);
}
.main::-webkit-scrollbar-track {/*滚动条里面轨道*/
  -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
  border-radius: 0;
  background: rgba(0,0,0,0.1);
}
</style>
