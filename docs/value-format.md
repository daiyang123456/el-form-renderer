```vue
<template>
  <div class="format">
    <el-form-renderer :content="content" inline ref="formRender">
      <el-form-item>
        <el-button @click="setValue">set value</el-button>
        <el-button type="primary" @click="getValue">log value</el-button>
      </el-form-item>
    </el-form-renderer>
  </div>
</template>

<script>
  export default {
    name: 'format',
    data() {
      return {
        content: [
          {
            el: {
              type: 'daterange',
              valueFormat: 'yyyy-MM-dd'
            },
            type: 'date-picker',
            id: 'date',
            label: 'date',
            inputFormat: (row) => {
              if (row.startDate && row.endDate) {
                return [row.startDate, row.endDate]
              }
            },
            outputFormat: (val) => {
              if (!val) {
                return {startDate: '', endDate: ''}
              }
              return {
                startDate: val[0],
                endDate: val[1]
              }
            }
          }
        ]
      }
    },
    methods: {
      getValue () {
        const value = this.$refs.formRender.getFormValue()
        console.log(value)  // 输出为对应id 和值组成的对象
      },
      setValue () {
        this.$refs.formRender.updateForm({
          startDate: '2019-01-01',
          endDate: '2019-01-02'
        })
      }
    }
  }
</script>
```
