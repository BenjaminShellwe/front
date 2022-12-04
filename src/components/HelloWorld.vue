<template>
  <div>
    <el-row>
      <el-col :span="6" style="margin-right: 5px; max-width: 470px;">
        <h1>{{ msg }}</h1>
        <p>
          Spring Boot(2.7[Java11]) Vue.js<br>
          日志查看工具(内部工具)<br>
          遇到问题与shellwe反馈<br>
          目前你只能找我本人反馈
          <el-divider />
          <span style="font-size: 22px; ">简易输出台：</span>
          <el-scrollbar
              id=""
              height="100px"
              style="margin-top: 20px; background-color: #aaaaaa"
              @click="getBackEndInformation"
          >
              此处返回LogReader的运行状态等信息
          </el-scrollbar>
        </p>
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
              <el-button type="primary" >查询</el-button>
              <el-button>清空所有</el-button>
            </el-form-item>
          </el-form>
        </el-card>

      </el-col>
      <el-col :span="18">
        <el-card shadow="hover">
          <template #header>
            <div class="card-header">
              <el-row>
                <el-col :span="2">
                  <h2 style="margin: 0">日志内容</h2>
                </el-col>
                <el-col :span="22">
                  <el-button @click="getWebSocket">刷新</el-button>
                </el-col>
              </el-row>
            </div>
          </template>
          <el-scrollbar height="600px">
            <div id="log-container" style="background: #333; color: #aaa; padding: 10px;" >
              此处展示日志内容 LogReader后台启动后自动刷新
            </div>
          </el-scrollbar>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import {reactive} from "vue";

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      form,
      getWebSocket
    }
  },
  mounted() {
    // WebSocket
    if ('WebSocket' in window) {
      this.websocket = new WebSocket('ws://localhost:8080/log')
      // alert('连接浏览器')
      this.initWebSocket()
    } else {
      alert('当前浏览器 不支持')
    }
  },
  beforeUnmount() {
    this.onbeforeunload()
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
      console.log('WebSocket连接发生错误   状态码：' + this.websocket.readyState)
    },
    setOnopenMessage () {
      console.log('WebSocket连接成功    状态码：' + this.websocket.readyState)
    },
    setOnmessageMessage (event) {
      // 根据服务器推送的消息做自己的业务处理
      console.log('服务端返回：' + event.data)
    },
    setOncloseMessage () {
      console.log('WebSocket连接关闭    状态码：' + this.websocket.readyState)
    },
    onbeforeunload () {
      this.closeWebSocket()
    },
    closeWebSocket () {
      this.websocket.close()
    }
  }
}
const form = reactive({
  msgid: '',
  traceno: '',
  date1: '',
  date2: '',
  delivery: false,
  type: [],
  resource: '',
  desc: '',
})


const getWebSocket = () => {
  let websocket = new WebSocket('ws://localhost:8080/log');
  websocket.onmessage = function(event) {
    console.log(event)
    document.getElementById("log-container").innerHTML = event.data
  }
}


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">

</style>
