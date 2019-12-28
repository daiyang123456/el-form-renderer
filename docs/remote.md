远程获取el-select、checkbox-group & radio-group 的options。

你甚至可以远程获取任意一个组件prop！

```vue
<template>
  <div>
      <el-button @click="open('ruleForm')">打开</el-button>
      <el-dialog
        width="30%"
        class="dialog-collect-bankcard"
        :before-close="close"
        :visible="showPopu"
        title="收集"
        top="90px"
        destroy-on-close
      >
        <el-form-renderer :content="content" />
      </el-dialog>
  </div>
</template>

<script>
export default {
  methods: {
    open() {
      this.showPopu = true
    },
    close() {
      this.showPopu = false
    },
  },
  data () {
    return {
      showPopu: false,
      content: [
        // 只需要设置 url，即可远程配置 options
        {
          type: 'select',
          id: 'select',
          label: 'select',
          remote: {
            url: 'https://mockapi.eolinker.com/IeZWjzy87c204a1f7030b2a17b00f3776ce0a07a5030a1b/el-form-renderer?q=remote',
          }
        },
        // 可以自定义 request 方法，做各种操作
        {
          type: 'radio-group',
          id: 'radio',
          label: 'radio',
          remote: {
            request() {
              const resp = {
                path: [
                  {title: 'resourceA', name: 1},
                  {title: 'resourceB', name: 2},
                ]
              }
              return new Promise(r => setTimeout(() => r(resp), 2000))
            },
            dataPath: 'path',
            label: 'title',
            value: 'name'
          }
        },
        // request 报错时也可以处理
        {
          type: 'checkbox-group',
          id: 'checkbox',
          label: 'checkbox',
          default: [],
          remote: {
            request() {
              // throw new Error('test')
              return Promise.reject(new Error('test2'))
            },
            onError: error => {
              console.warn(error.message)
              return [
                {label: 'typeA'},
                {label: 'typeB'},
                {label: 'typeC'},
              ]
            }
          }
        },
        // 你想远程配置任何 prop 都行！
        {
          type: 'cascader',
          id: 'cascader',
          label: 'cascader',
          default: [],
          remote: {
            prop: 'options',
            request() {
              return [
                {
                  label: 'hello', 
                  value: 'hello', 
                  children: [
                    {
                      label: 'world',
                      value: 'world',
                    }
                  ]
                },
              ]
            },
          }
        },
      ]
    }
  }
}
</script>
```
