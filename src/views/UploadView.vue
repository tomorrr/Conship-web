<!--
 * @Author: wangbyyds 1362872827@qq.com
 * @Date: 2022-06-19 22:37:30
 * @LastEditors: wangbyyds 1362872827@qq.com
 * @LastEditTime: 2022-07-08 08:38:15
 * @FilePath: \Conship-web\src\views\UploadView.vue
 * @Description: 
 * 
 * Copyright (c) 2022 by wangbyyds 1362872827@qq.com, All Rights Reserved. 
-->
<template>
  <div class="home">
    <MyChose />
    <div class="main">
      <MonacoEditor
        width="550"
        height="400"
        theme="vs-dark"
        language="javascript"
        :options="options"
        @change="onChange"
      ></MonacoEditor>
      <div class="bottom">
        <Select :selectData="selectData" :selValue="selValue" color="white" @getValue="getValue" />
        <router-link class="button middle plain" to="/success/URL/SECRET" @click.native="postUpload">提交</router-link>
      </div>
    </div>

    <!-- <img alt="Vue logo" src="../assets/logo.png" /> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import MyChose from '@/components/MyChose.vue'
// import MyHelp from '@/components/MyHelp.vue'
import MonacoEditor from 'monaco-editor-vue'
// import incoder from '@/components/vue-codemirror.vue'
import Select from '@/components/SelectFormat.vue'
// import MyButton from '@/components/MyButton.vue'

//按需导入
import { PostHomeAPI } from '@/request/api.js'

export default {
  name: 'HomeView',
  // components: {
  //   HelloWorld,
  // },

  components: {
    MyChose,
    // MyHelp,
    MonacoEditor,
    // incoder,
    Select,
    // MyButton,
  },
  data() {
    return {
      options: {
        value: '', // 初始值
        foldingStrategy: 'indentation', // 代码可分小段折叠
        automaticLayout: true, // 自适应布局
        overviewRulerBorder: false, // 不要滚动条的边框
        autoClosingBrackets: true,
        tabSize: 2, // tab 缩进长度
        minimap: {
          enabled: false, // 不要小地图
        },
      },
      //Monaco Editor Options
      selectData: [
        {
          name: 'json',
          value: 11,
        },
        {
          name: 'yaml',
          value: 12,
        },
        {
          name: 'toml',
          value: 13,
        },
        {
          name: 'sh',
          value: 14,
        },
      ],
      selValue: '',
    }
  },
  created() {
    // 初始化下拉框
    this.selValue = this.selectData[0].value
  },
  methods: {
    onChange(value) {
      this.options.value = value
    },
    getValue(name, value, index) {
      console.log('item:', name, value, index)
    },
    onEditorMounted(editor, monaco) {
      window.editor = editor
      window.monaco = monaco
    },
    postUpload() {
      alert('傻子它调用了')
      PostHomeAPI({
        content: this.options.value,
      }).then((res) => {
        // console.log(res)
        // console.log(this.onChange)
        // console.log(this.options.value)
        // alert(res.data), alert(res.data.data.url), alert(res.data.data.secret)
        this.$router.push({
          path: '/success',
          query: {
            url: res.data.data.url,
            secret: res.data.data.secret,
          },
        })
      })
    },
  },
  // methods: {
  //   /**
  //    * 数据加载
  //    */
  //   initHandler() {
  //     this.initEditor()
  //   },
  //   // 初始化编辑器，确保dom已经渲染
  //   initEditor() {
  //     // 编辑器配置项 参考文档https://microsoft.github.io/monaco-editor/api/modules/monaco.html
  //     // 自定义灰色背景主题
  //     monaco.editor.defineTheme('GreyTheme', {
  //       base: 'vs',
  //       inherit: true,
  //       rules: [{ background: '#F9F9F9' }],
  //       colors: {
  //         // 相关颜色属性配置
  //         'editor.background': '#F9F9F9', //背景色
  //       },
  //     })
  //     //设置自定义主题
  //     monaco.editor.setTheme('GreyTheme')
  //     // 下面属性设置等同于官网DEMO效果
  //     this.editor = monaco.editor.create(document.getElementById('editor'), {
  //       value: this.value, //编辑器初始显示文字
  //       language: this.options.language, //语言支持自行查阅demo
  //       automaticLayout: false, //自动布局
  //       theme: 'GreyTheme', //官方自带三种主题vs, hc-black, or vs-dark
  //       selectOnLineNumbers: true, //显示行号
  //       roundedSelection: false,
  //       readOnly: false, // 只读
  //       cursorStyle: 'line', //光标样式
  //       glyphMargin: true, //字形边缘
  //       useTabStops: false,
  //       fontSize: this.options.fontSize, //字体大小
  //       autoIndent: false, //自动布局
  //       renderWhitespace: 'all',
  //       colorDecorators: true,
  //       matchBrackets: 'always',
  //       accessibilitySupport: 'on',
  //       suggestions: true,
  //       snippetSuggestions: 'top',
  //     })
  //   },
  //   /**
  //    * 获取编辑器值
  //    * @returns {*}
  //    */
  //   getValue() {
  //     return this.editor.getValue() //获取编辑器中的文本
  //   },
  // },
  // destroyed() {
  //   this.$emit('getEditorValue', this.editor.getValue())
  // },
}
</script>

<style>
/*清除默认样式*/

* {
  margin: 0;
  padding: 0;
}

li {
  list-style: none;
}

img {
  vertical-align: top;
  border: none;
}

.home {
  display: flex;
  justify-content: center;
  align-items: flex-start;
}

/* main框 */

.main {
  width: 600px;
  height: 500px;
  padding-top: 20px;
  margin-left: 20px;
  border: 2px black solid;
  border-radius: 20px;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
}
/* bottom底边div */
.bottom {
  width: 90%;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}

.button {
  appearance: none;
  border: none;
  outline: none;
  background: #fff;
  text-align: center;
  border: 1px solid transparent;
  border-radius: 4px;
  cursor: pointer;
  text-decoration: none;
}

.middle {
  width: 120px;
  height: 40px;
  font-size: 17px;
  border-radius: 10px;
}

.plain {
  border-color: #27ba9b;
  color: #27ba9b;
  background: #e6faf6;
}
</style>
