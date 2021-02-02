<template>
  <el-card shadow="always" :body-style="{padding: '0px'}">
    <el-row>
      <el-col :span="3">
        <el-image
          style="height: 80px"
          fit="contain"
          :src="data.icon"
        />
        <div class="center">
          <h4>
            属性
          </h4>
          {{ data.attribute }}
        </div>
      </el-col>

      <el-col :span="21">
        <h3 v-if="data.name != ''" class="name">
          {{ data.name }}
        </h3>
        <h3 v-else class="name">
          未
        </h3>
        <div>
          {{ data.description }}
        </div>
        <div class="elem">
          <span :class="{ 'red': data.requiredColour === '赤', 'blue': data.requiredColour === '青', 'yellow': data.requiredColour === '黄', green: data.requiredColour === '緑', purple: data.requiredColour === '紫' }">
            必要な音符数: {{ `${data.requiredColour}${data.requiredNumber}` }}
          </span>
        </div>
        <div class="elem">
          <span v-if="data.range">
            効果範囲:{{ data.range }}
          </span>
          <span v-if="data.change">
            変化数:{{ data.change }}個
          </span>
          <span v-if="data.time">
            効果時間:{{ data.time }}秒
          </span>
        </div>
      </el-col>
    </el-row>
  </el-card>
</template>

<script>
export default {
  props: {
    data: {
      type: Object,
      require: true
    }
  },
  data () {
    return {
    }
  }
}
</script>

<style scoped>
.name {
  margin-bottom: 5px;
}
.red{
  border-bottom:solid;
  border-color:#ff0000;
  border-width:5px;
}
.blue{
  border-bottom:solid;
  border-color:#0000ff;
  border-width:5px;
}
.yellow{
  border-bottom:solid;
  border-color:#ffff00;
  border-width:5px;
}
.green{
  border-bottom:solid;
  border-color:#008000;
  border-width:5px;
}
.purple{
  border-bottom:solid;
  border-color:#800080;
  border-width:5px;
}
.elem {
  text-indent:1em;
  margin-top: 10px;
  font-size: 90%;
}
.center {
  text-align: center;
}
</style>
