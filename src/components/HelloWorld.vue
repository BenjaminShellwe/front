<template>
  <div>
    <h1>{{ msg }}</h1>
    <p>
      由Spring Boot(3.0), Vue.js开的日志查看工具(内部工具)<br>
    </p>
    <el-row>
      <el-col :span="6" style="margin-right: 5px; max-width: 470px;">
        <el-card>
          <template #header>
            <div class="card-header">
              <span>模块一</span>
              <el-button class="button" text>Operation button</el-button>
            </div>
          </template>
          <div v-for="o in 9" :key="o" class="text item">{{ 'List item ' + o }}</div>

        </el-card>
      </el-col>
      <el-col :span="18">
        <el-card>
          <template #header>
            <div class="card-header">
              <span>模块二</span>
              <el-button class="button" text>Operation button</el-button>
            </div>
          </template>
          <div v-for="o in 4" :key="o" class="text item">{{ 'List item ' + o }}</div>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">

</style>
