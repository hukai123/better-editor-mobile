<template>
  <div class="test-container flex">
    <div>这是test页面</div>
    <div>这是test页面</div>
    <div>这是test页面</div>
    <div>这是test页面</div>
    <div>这是test页面</div>
    <div>这是test页面</div>
    <div>这是test页面</div>
    <div>这是test页面</div>
    <div>这是test页面</div>
    <div@click="aa">这是test页面</div@click="aa">
    <div @click="aa">这是test页面</div>
    <div class="test-box">
      <h-editor ref="test3" @change="htmlChange"></h-editor>
      <div id="editor2"></div>
      
    </div>
    
  </div>
</template>
<script>
import E from 'better-editor';
import E2 from 'wangeditor';
export default {
  name: 'test',
  data() {
    return {
      editor: null,
      editor2: null,
    }
  },
  components: {
   
  },
  mounted() {

    // this.initEditor2()
  },
  methods: {
    htmlChange(e){
      console.log(e);
    },
    aa() {
      let html = this.$refs.test3.getHtml();
      console.log(html);
    },
    initEditor() {
      // http://192.168.2.125:8080/admin/storage/create
      this.editor = new E(this.$refs.toolbar, this.$refs.editor)
      this.editor.customConfig.uploadImgShowBase64 = false // base 64 存储图片
      this.editor.customConfig.uploadImgServer = 'http://otp.cdinfotech.top/file/upload_images'// 配置服务器端地址
      this.editor.customConfig.uploadImgHeaders = { }// 自定义 header
      this.editor.customConfig.uploadFileName = 'file' // 后端接受上传文件的参数名
      this.editor.customConfig.uploadImgMaxSize = 2 * 1024 * 1024 // 将图片大小限制为 2M
      this.editor.customConfig.uploadImgMaxLength = 6 // 限制一次最多上传 3 张图片
      this.editor.customConfig.uploadImgTimeout = 3 * 60 * 1000 // 设置超时时间

      // 配置菜单
      this.editor.customConfig.menus = [
        'head', // 标题
        'bold', // 粗体
        'fontSize', // 字号
        'fontName', // 字体
        'italic', // 斜体
        'underline', // 下划线
        'strikeThrough', // 删除线
        'foreColor', // 文字颜色
        'backColor', // 背景颜色
        'link', // 插入链接
        'list', // 列表
        'justify', // 对齐方式
        'quote', // 引用
        'emoticon', // 表情
        'image', // 插入图片
        'table', // 表格
        'video', // 插入视频
        'code', // 插入代码
        'undo', // 撤销
        'redo', // 重复
        'fullscreen' // 全屏
      ]

      this.editor.customConfig.uploadImgHooks = {
        fail: (xhr, editor, result) => {
          // 插入图片失败回调
        },
        success: (xhr, editor, result) => {
          // 图片上传成功回调
        },
        timeout: (xhr, editor) => {
          // 网络超时的回调
        },
        error: (xhr, editor) => {
          // 图片上传错误的回调
        },
        customInsert: (insertImg, result, editor) => {
          // 图片上传成功，插入图片的回调
          //result为上传图片成功的时候返回的数据，这里我打印了一下发现后台返回的是data：[{url:"路径的形式"},...]
          // console.log(result.data[0].url)
          //insertImg()为插入图片的函数
            //循环插入图片
          // for (let i = 0; i < 1; i++) {
            // console.log(result)
            let url = "http://otp.cdinfotech.top"+result.url
            insertImg(url)
          // }
        }
      }
      this.editor.customConfig.onchange = (html) => {
        this.info_ = html // 绑定当前逐渐地值
        this.$emit('change', this.info_) // 将内容同步到父组件中
      }
      // 创建富文本编辑器
      this.editor.create()
    },
    initEditor2(){
      // this.editor2 = new E2('#editor2');
      // // this.editor2.customConfig.uploadImgShowBase64 = false // base 64 存储图片
      // this.editor2.customConfig.uploadImgServer = 'http://otp.cdinfotech.top/file/upload_images'// 配置服务器端地址
      // this.editor2.customConfig.uploadImgHeaders = { }// 自定义 header
      // this.editor2.customConfig.uploadFileName = 'file' // 后端接受上传文件的参数名
      // this.editor2.customConfig.uploadImgMaxSize = 2 * 1024 * 1024 // 将图片大小限制为 2M
      // this.editor2.customConfig.uploadImgMaxLength = 6 // 限制一次最多上传 3 张图片
      // this.editor2.customConfig.uploadImgTimeout = 3 * 60 * 1000 // 设置超时时间
      // // 创建富文本编辑器
      // this.editor2.create()
    }
  }
}
</script>
<style lang="less" scoped>
.test-container{
  min-height: 100%;
  // background:linear-gradient(180deg,rgba(54,67,111,1) 0%,rgba(31,35,68,1) 100%);
  // color: #fff;
  flex-direction: column;
  .test-box{
    padding: 50px;
    .editor-toolbar {
      border: 1px solid #ccc;
      background-color: #f1f1f1;
    }
    .editor-text {
      border: 1px solid #ccc;
      border-top: none;
      // min-height: 500px;
    }
  }
}
</style>
