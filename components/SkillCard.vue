<template>
  <div>
    <el-card shadow="always" :body-style="{padding: '0px'}" @click.native="dialogTableVisible=true">
      <el-row>
        <el-col :span="3">
          <div class="center">
            <el-image
              style="max-width: 80px"
              fit="contain"
              :src="data.icon"
            >
              <div slot="error">
                画像なし
              </div>
            </el-image>
            <h4>
              属性
            </h4>
            <div v-if="data.attribute">
              <span v-if="data.attribute == 'ALL'">ALL</span>
              <el-image
                v-else
                style="max-width: 30px"
                fit="contain"
                :src="attributeImgUrl[data.attribute]"
              />
            </div>
          </div>
        </el-col>

        <el-col :span="21">
          <h3 v-if="data.name" class="name">
            {{ data.name }}
          </h3>
          <h3 v-else class="name">
            未登録
          </h3>
          <div class="elem" >
            <span>
              {{ data.description }}
            </span>
          </div>
          <div class="elem">
            <span :class="{ 'red-line': data.requiredColor === '赤', 'blue-line': data.requiredColor === '青', 'yellow-line': data.requiredColor === '黄', 'green-line': data.requiredColor === '緑', 'purple-line': data.requiredColor === '紫' }">
              必要な音符数: {{ `${data.requiredColor}${data.requiredNumber}` }}
            </span>
          </div>
          <div class="elem">
            <span v-if="data.range">
              <table border="1" align="left">
                <tr>
                  <th class="bold">Lv<th>
                  <th v-for="(row, index) in data.range.split(',')" :key="index">{{ index+1 }}</th>
                </tr>
                <tr>
                  <th class="bold">効果範囲<th>
                  <th v-for="(row, index) in data.range.split(',')" :key="index">{{ row }}</th>
                </tr>
              </table>
            </span>
            <span v-if="data.change">
              <table border="1" align="left">
                <tr>
                  <th class="bold">Lv<th>
                  <th v-for="(row, index) in data.change.split(',')" :key="index">{{ index+1 }}</th>
                </tr>
                <tr>
                  <th class="bold">変化数<th>
                  <th v-for="(row, index) in data.change.split(',')" :key="index">{{ row }}</th>
                </tr>
              </table>
            </span>
            <span v-if="data.time">
              <table border="1" align="left">
                <tr>
                  <th class="bold">Lv<th>
                  <th v-for="(row, index) in data.time.split(',')" :key="index">{{ index+1 }}</th>
                </tr>
                <tr>
                  <th class="bold">変化数<th>
                  <th v-for="(row, index) in data.time.split(',')" :key="index">{{ row }}</th>
                </tr>
              </table>
            </span>
            <span v-if="data.occurrence">
              <table border="1" align="left">
                <tr>
                  <th class="bold">Lv<th>
                  <th v-for="(row, index) in data.occurrence.split(',')" :key="index">{{ index+1 }}</th>
                </tr>
                <tr>
                  <th class="bold">変化数<th>
                  <th v-for="(row, index) in data.occurrence.split(',')" :key="index">{{ row }}</th>
                </tr>
              </table>
            </span>
            <span v-if="data.erasure">
              <table border="1" align="left">
                <tr>
                  <th class="bold">Lv<th>
                  <th v-for="(row, index) in data.erasure.split(',')" :key="index">{{ index+1 }}</th>
                </tr>
                <tr>
                  <th class="bold">変化数<th>
                  <th v-for="(row, index) in data.erasure.split(',')" :key="index">{{ row }}</th>
                </tr>
              </table>
            </span>
          </div>
        </el-col>
      </el-row>
    </el-card>
    <el-dialog :title="`${data.name}対応アイドル・属性一覧`" :visible.sync="dialogTableVisible">
      <el-table :data="filteredIdols">
        <el-table-column property="name" label="名前"></el-table-column>
        <el-table-column property="attribute1" label="属性1">
          <template slot-scope="scope">
            <el-image style="max-width: 40px" :src="attributeImgUrl[scope.row.attribute1]" />
          </template>
        </el-table-column>
        <el-table-column property="attribute2" label="属性2">
          <template slot-scope="scope">
            <el-image style="max-width: 40px" :src="attributeImgUrl[scope.row.attribute2]" />
          </template>
        </el-table-column>
      </el-table>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data () {
    return {
      dialogTableVisible: false
    }
  },
  props: {
    data: {
      type: Object,
      require: true
    },
    attributeImgUrl: {
      type: Object,
      require: true
    },
    idolData: {
      type: Array,
      require: true
    }
  },
  computed: {
    filteredIdols () {
      if (this.data.attribute === 'ALL') {
        return this.idolData
      }
      const res = []
      for (const idol of this.idolData) {
        if (idol.attribute1 === this.data.attribute) {
          res.push({
            name: idol.name,
            attribute1: idol.attribute1,
            attribute2: idol.attribute2
          })
        } else if (idol.attribute2 === this.data.attribute) {
          res.push({
            name: idol.name,
            attribute1: idol.attribute2,
            attribute2: idol.attribute1
          })
        }
      }
      return res
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
  border-color:red !important;

  border-width:5px;
}
.blue-line{
  border-left:solid;
  border-color:blue;
  border-width:5px;
}
.yellow-line{
  border-left:solid;
  border-color:yellow !important;
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
.bold {
  font-weight: bold;
}
</style>
