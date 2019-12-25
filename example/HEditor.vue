<template>
  <div class="h-editor">
    <div ref="toolbar" class="editor-toolbar">
    </div>
    <div ref="editor" class="editor-text"  :style="{height: height+'px'}">
    </div>
  </div>
</template>

<script>
import { getData } from "@/api";
import E from 'better-editor'
export default {
  name: 'editoritem',
  data() {
    return {
      // uploadPath,
      editor: null,
      info_: null
    }
  },
  model: {
    prop: 'value',
    event: 'change'
  },
  props: {
    value: {
      type: String,
      default: ''
    },
    isClear: {
      type: Boolean,
      default: false
    },
    height: {
      type: String,
      default: '400'
    }
  },
  watch: {
    isClear(val) {
      if (val) {
        this.editor.txt.clear()
        this.info_ = null
      }
    },
    value: function(value) {
      if (value !== this.editor.txt.html()) {
        this.editor.txt.html(this.value)
      }
    }
  },
  created() {
    // this.getOSSKey();
  },
  mounted() {
    this.initEditor()
    this.editor.txt.html(this.value)
  },
  methods: {
    getOSSKey() {
      getData.getOSSKey().then(res => {
        this.OSSKey = res;
      })
      .catch(e => {
        console.log("获取oss签名失败", e);
        if (e.code != 401) {
        }
      });
    },
    initEditor() {
      let that = this;
      this.editor = new E(this.$refs.toolbar, this.$refs.editor)
      this.editor.customConfig.customUploadImg = function(files, insert) {
        let formData = new FormData();
        formData.append("file", files[0]);
        getData.uploadFile(formData).then(res => {
          insert(res);
        })
        .catch(err => {
          if (err && err.msg) {
            this.openMessage(0, err.msg);
          } else {
            this.openMessage(0, "网速太慢了");
            console.log(err);
          }
        });
      };
      this.editor.customConfig.customUploadVideo = function(files, insert) {
        let formData = new FormData();
        formData.append("file", files[0]);
        getData.uploadFile(formData).then(res => {
          insert(res);
        })
        .catch(err => {
          if (err && err.msg) {
            that.openMessage(0, err.msg);
          } else {
            that.openMessage(0, "网速太慢了");
            console.log(err);
          }
        });
      };
      this.editor.customConfig.onchange = html => {
        this.info_ = html // 绑定当前逐渐地值
        that.$emit('change', this.info_) // 将内容同步到父组件中
      };
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
      this.editor.create();
    },
    getHtml() {
      return this.editor.txt.html();
    }
  }
}
</script>

<style lang="less">
.h-editor {
  width: 100%;
  margin: 0 auto;
  position: relative;
  z-index: 0;
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
// .editor-toolbar {
//   border: 1px solid #ccc;
//   background-color: #f1f1f1;
// }
// .editor-text {
//   border: 1px solid #ccc;
//   border-top: none;
//   // min-height: 500px;
// }
</style>