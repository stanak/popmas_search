<template>
  <div>
    <div class="summary">
      <h2>ポプマスデータ検索</h2>
      <p class="margin">
        ポプマスのデータを多様な絞り込みで一覧します。スキルの並び順はゲーム内表示順を属性順に指定したときと同じです。スキル部分をクリックすると、そのスキルを使えるアイドル名・アイドルの属性一覧が出てきます。
      </p>
      <p class="margin">
        スキルとマイクの組み合わせの最大化を検討する等、ユニット結成の参考にどうぞ。何かの役に立ったら<a href="https://seesaawiki.jp/aiko_takamori/">高森藍子</a>を少し応援してやってください。
      </p>
      <p class="margin">
        データである<a href="https://docs.google.com/spreadsheets/d/1DnNpbOu1PoKKkuc1-gfaf842pACKGAG19NOFdJkSJrk/edit#gid=0">スプレッドシート</a>と<a href="https://drive.google.com/drive/folders/1JNlFbd7RJ9O5OlBso6VZY-pT3NCVCS_A?usp=sharing">Drive</a>の編集は誰でもできます。要望とか報告等あれば<a href="https://twitter.com/_starnak">@_starnak</a>まで。
      </p>
      <p class="margin">
        技術的には、<a href="https://docs.google.com/spreadsheets/d/1DnNpbOu1PoKKkuc1-gfaf842pACKGAG19NOFdJkSJrk/edit#gid=0">スプレッドシート</a>をapps scriptで<a :href="resource">json</a>に変換しています。このページのソースコードは<a href="https://github.com/stanak/popmas_search">ここ</a>です。
      </p>
    </div>
    <el-form ref="form" :model="form" label-width="120px" class="form">
      <el-form-item label="名前">
        <el-input v-model="form.name" class="input" />
      </el-form-item>
      <el-form-item label="説明">
        <el-input v-model="form.description" class="input" />
      </el-form-item>
      <el-form-item label="属性">
        <el-select multiple v-model="form.attribute" placeholder="属性">
          <el-option
            v-for="item in attributes"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="必要色">
        <el-select multiple v-model="form.requiredColor" placeholder="必要色">
          <el-option
            v-for="item in colors"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button @click="clear()">
          検索条件クリア
        </el-button>
      </el-form-item>
    </el-form>
    <div v-for="(data, index) in filteredSkillData" :key="index">
      <SkillCard :data="data" :attributeImgUrl="attributeImgUrl" :idolData="idolData"/>
    </div>
  </div>
</template>

<script>
import { jsonp } from 'vue-jsonp'

const resource = 'https://script.google.com/macros/s/AKfycbysa8wIgXc3CIMkzYTUdWOA-gjRQKmtZpFyTo4xeMCXwH2u2VJZqb3TdA/exec'

// mutipleなselectの実体はただのArrayじゃなかったので、str in arrayが効かず
const multipleSelectObjHas = (multipleSelectObj, str) => {
  for (const attr of multipleSelectObj) {
    if (attr === str) { return true }
  }
  return false
}

export default {
  component: {
    SkillCard: () => import('~/components/SkillCard')
  },
  async asyncData () {
    const response = await jsonp(resource, { callbackName: 'callbackFunction' })
    return {
      skillData: response.skill,
      attributeImgUrl: response.attribute,
      idolData: response.idol
    }
  },
  data () {
    return {
      resource,
      form: {
        name: '',
        description: '',
        attribute: [],
        requiredColor: []
      },
      skillData: {},
      attributeImgUrl: {},
      attributes: [
        {
          value: 'ALL',
          label: 'ALL'
        },
        {
          value: '花',
          label: '花'
        },
        {
          value: '空',
          label: '空'
        },
        {
          value: '炎',
          label: '炎'
        },
        {
          value: '風',
          label: '風'
        },
        {
          value: '雪',
          label: '雪'
        },
        {
          value: '月',
          label: '月'
        },
        {
          value: '天',
          label: '天'
        },
        {
          value: '夢',
          label: '夢'
        },
        {
          value: '虹',
          label: '虹'
        },
        {
          value: '雷',
          label: '雷'
        },
        {
          value: '光',
          label: '光'
        },
        {
          value: '星',
          label: '星'
        },
        {
          value: '海',
          label: '海'
        },
        {
          value: '愛',
          label: '愛'
        },
        {
          value: '闇',
          label: '闇'
        }
      ],
      colors: [
        {
          value: '赤',
          label: '赤'
        },
        {
          value: '青',
          label: '青'
        },
        {
          value: '黄',
          label: '黄'
        },
        {
          value: '緑',
          label: '緑'
        },
        {
          value: '紫',
          label: '紫'
        }
      ]
    }
  },
  computed: {
    filteredSkillData () {
      return this.skillData.filter((data) => {
        const matchName = this.form.name ? data.name.includes(this.form.name) : true
        const matchDescription = this.form.description ? data.description.includes(this.form.description) : true
        const matchAttribute = this.form.attribute.length ? multipleSelectObjHas(this.form.attribute, data.attribute) : true
        const matchRequiredColor = this.form.requiredColor.length ? multipleSelectObjHas(this.form.requiredColor, data.requiredColor) : true
        return matchName && matchDescription && matchAttribute && matchRequiredColor
      })
    }
  },
  methods: {
    clear () {
      this.form = {
        name: '',
        description: '',
        attribute: '',
        requiredColor: ''
      }
    }
  }
}
</script>

<style scoped>
.form {
  margin: 50px;
}
.input {
  width: 100%;
}
.summary {
  margin: 50px;
}
.margin {
  margin: 10px;
}
</style>
