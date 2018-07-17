<template>
  <el-cascader
    :props="props"
    :options="selectData"
    change-on-select
    clearable
    @change="loadAreaDict"
    v-model="fValue"
  ></el-cascader>
</template>

<script>
  export default {
    name: 'areaselect',
    props: {
      value: {
        type: Array,
        default () {
          return []
        }
      },
      level: {
        type: Number,
        default: 3
      }
    },
    data () {
      return {
        fValue: this.value,
        selectData: [],
        props: {
          value: 'area_id',
          label: 'area_name'
        }
      }
    },
    computed: {
      url () {
        return this.$store.getters.gApiUrl
      }
    },
    watch: {
      fValue (val) {
        this.$emit('input', val)
      },
      value (val) {
        this.fValue = val
      }
    },
    created () {
      console.log(this.value)
      if (!this.value.length || this.value.length === 1) {
        this.loadAreaDict()
      } else if (this.value.length === 2) {
        this.loadAreaDict().then(() => {
          this.loadAreaDict(this.value[0])
        })
      } else if (this.value.length === 3) {
        this.loadAreaDict().then(() => {
          this.loadAreaDict([this.value[0]]).then(() => {
            this.loadAreaDict([this.value[0], this.value[1]])
          })
        })
      }
    },
    methods: {
      loadAreaDict (areaArr = []) {
        if (areaArr.length === this.level) return
        return this.axios.get(this.url + '/basic/dictionary/getarea', {
          params: {
            area_id: areaArr[areaArr.length - 1]
          }
        }).then(res => {
          switch (areaArr.length) {
            case 0:
              this.selectData = res.map(data => {
                data.children = []
                return data
              })
              return
            case 1:
              this.selectData.filter(data => data.area_id === areaArr[0])[0].children = res.map(data => {
                data.children = []
                return data
              })
              return
            case 2:
              this.selectData.filter(data => data.area_id === areaArr[0])[0].children.filter(data => data.area_id === areaArr[1])[0].children = res
          }
        })
      }
    }
  }
</script>
