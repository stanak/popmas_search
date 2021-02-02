<template>
  <div>
    <el-form ref="form" :model="form" label-width="120px" class="form">
      <el-form-item label="名前">
        <el-input v-model="form.name"></el-input>
      </el-form-item>
      <el-form-item label="説明">
        <el-input v-model="form.description"></el-input>
      </el-form-item>
      <el-form-item label="属性">
        <el-select v-model="form.attribute" placeholder="属性">
          <el-option
            v-for="item in attributes"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="必要色">
        <el-select v-model="form.requiredColour" placeholder="必要色">
          <el-option
            v-for="item in colours"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button @click="clear()">Clear</el-button>
      </el-form-item>
    </el-form>
    <div v-for="(data, index) in filteredSkillData" :key="index">
      <SkillCard :data="data" />
    </div>
  </div>
</template>

<script>
const resource = 'https://script.google.com/macros/s/AKfycbysa8wIgXc3CIMkzYTUdWOA-gjRQKmtZpFyTo4xeMCXwH2u2VJZqb3TdA/exec'

export default {
  component: {
    SkillCard: () => import('~/components/SkillCard')
  },
  data () {
    return {
      form: {
        name: '',
        description: '',
        attribute: '',
        requiredColour: ''
      },
      skillData: {},
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
      colours: [
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
  async asyncData ({ $axios }) {
    const response = await $axios.$get(resource)
    return {
      skillData: response
    }
  },
  methods: {
    clear () {
      this.form = {
        name: '',
        description: '',
        attribute: '',
        requiredColour: ''
      }
    }
  },
  computed: {
    filteredSkillData () {
      return this.skillData.filter((data) => {
        const matchName = this.form.name ? data.name.includes(this.form.name) : true
        const matchDescription = this.form.description ? data.description.includes(this.form.description) : true
        const matchAttribute = this.form.attribute ? this.form.attribute === data.attribute : true
        const matchRequiredColour = this.form.requiredColour ? this.form.requiredColour === data.requiredColour : true
        return matchName && matchDescription && matchAttribute && matchRequiredColour
      })
    }
  }
}
</script>

<style scoped>
.form {
  margin: 10px;
}
</style>
