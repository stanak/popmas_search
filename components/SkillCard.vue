<template>
  <el-card shadow="always" :body-style="{padding: '0px'}">
    <el-row>
      <el-col :span="3">
        <div class="center">
          <el-image
            style="max-width: 80px"
            fit="contain"
            :src="data.icon"
          />
          <h4>
            属性
          </h4>
          <span v-if="data.attribute == 'ALL'">ALL</span>
          <el-image
            v-else
            style="max-width: 30px"
            fit="contain"
            :src="attributeImgUrl[data.attribute]"
          />
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
          <span :class="{ 'red-line': data.requiredColor === '赤', 'blue-line': data.requiredColor === '青', 'yellow-line': data.requiredColor === '黄', 'green-line': data.requiredColor === '緑', 'purple-line': data.requiredColor === '紫' }">
            必要な音符数: {{ `${data.requiredColor}${data.requiredNumber}` }}
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
    },
    attributeImgUrl: {
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
.red-line{
  border-left:solid;
  border-color:red;
  border-width:5px;
}
.blue-line{
  border-left:solid;
  border-color:blue;
  border-width:5px;
}
.yellow-line{
  border-left:solid;
  border-color:yellow;
  border-width:5px;
}
.green-line{
  border-left:solid;
  border-color:green;
  border-width:5px;
}
.purple-line{
  border-left:solid;
  border-color:purple;
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
