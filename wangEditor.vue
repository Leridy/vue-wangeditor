<template>
  <div id="Content">
  </div>
</template>

<script>
  import E from 'wangeditor';

  export default {
    name: 'wangEditor',
    props: ['uploadHost', 'value', 'uploadFileName'],
    model: {
      prop: 'value',
      event: 'change',
    },
    data() {
      return {
        valueBack: '1',
      };
    },
    methods: {
      initEditor() {
        const self = this;
        const editor = new E('#Content');
        editor.customConfig.uploadImgServer = self.uploadHost;
        editor.customConfig.uploadImgMaxSize = 5 * 1024 * 1024;
        editor.customConfig.customAlert = () => {
          // info 是需要提示的内容
          // self.$Message.info(info);
        };
        // editor.customConfig.debug = true;
        editor.customConfig.menus = [
          'head', 'bold',
          'italic',
          'underline',
          'strikeThrough',
          'foreColor',
          'backColor',
          'link',
          'list',
          'justify',
          'quote',
          'emoticon',
          'image',
          'table', // 'video',
          'code',
          'undo',
          'redo',
        ];
        editor.customConfig.onchange = self.onEditorChange;
        editor.customConfig.onblur = self.onEditorChange;
        editor.customConfig.uploadFileName = this.uploadFileName;
        editor.customConfig.uploadImgHooks = {
          before() {},
          fail(x, e, r) {
            const imgDom = `<img src="${r.url}" style="width: 100%">`;
            e.txt.append(imgDom);
          },
        };
        editor.create();
        self.editor = editor;
      },
      onEditorChange(value) {
        const self = this;
        self.valueBack = value;
        self.$emit('change', value);
      },
    },
    mounted() {
      const self = this;
      self.initEditor();
    },
    watch: {
      value(val) {
        const self = this;
        if (val !== self.valueBack) {
          self.editor.txt.clear();
          self.editor.txt.html(val);
        }
      },
      valueBack(val) {
        if (val === '') {
          self.editor.txt.clear();
        }
      },
    },
  };
</script>

<style scoped>

</style>
