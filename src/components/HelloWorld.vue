<template>
  <div>
    <el-row>
      <el-col :span="6" style="margin-right: 5px; max-width: 470px;">
        <h1>{{ msg }}</h1>
        <p>
          Spring Boot(3.0[Java17]) Vue.js<br>
          日志查看工具(内部工具)<br>
          遇到问题与shellwe反馈<br>
          <el-divider />
          简易输出台：
          <el-scrollbar height="100px">
            输出：一切正常，因为目前没有后台<br>
            输出：一切正常，因为目前没有后台<br>
            输出：一切正常，因为目前没有后台<br>
            输出：一切正常，因为目前没有后台<br>
            输出：一切正常，因为目前没有后台<br>
            输出：一切正常，因为目前没有后台<br>


            情况：未链接Linux<br>
            错误：检查网络<br>
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
                  <el-button>清屏</el-button>
                </el-col>
              </el-row>
            </div>
          </template>
          <el-scrollbar height="600px">
            <div v-for="o in 58" :key="o" class="text item">{{ 'List item ' + o }}</div>
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
      form
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
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">

</style>
