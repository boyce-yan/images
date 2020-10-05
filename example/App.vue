<template>
<el-card header="上传图片" shadow="never" style="max-width: 1250px;margin: 20px auto;">
    <ele-form :form-data="formData" :form-desc="formDesc" :request-fn="handleRequest" @request-success="handleSuccess" />
    <el-card class="box-card" v-show="show">
        <div v-for="o in 1" :key="o" class="text item">
            URL : <el-link type="primary">{{url}}</el-link> &nbsp;&nbsp;
            <el-button type="primary" plain @click="copy()">复制</el-button>
        </div>
    </el-card>

</el-card>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      show: false,
      url: '',
      formData: {},
      formDesc: {
        covers: {
          label: '封面',
          type: 'image-uploader',
          attrs: {
            drag: true, // 多张
            action: 'http://api.yanxiaolong.cn/qiniu/uploadImg',
            responseFn (response, file) {
              return 'http://image.yanxiaolong.cn/' + response.data
            }
          }
        }
      }
    }
  },
  methods: {
    handleRequest (data) {
      console.log(data)
      this.url = data.covers
      if (data.covers != null) {
        this.show = true
      }

      return Promise.resolve()
    },
    handleSuccess (data) {
      this.$message.success('提交成功')
    },
    copy () {
      let oInput = document.createElement('input')
      oInput.value = this.url
      document.body.appendChild(oInput)
      oInput.select() // 选择对象;

      document.execCommand('Copy') // 执行浏览器复制命令
      this.$message({
        message: '复制成功',
        type: 'success'
      })
      oInput.remove()
    }
  },
  mounted () {}
}
</script>

<style>
body {
    background-color: #f0f2f5;
}

.box-card {
    width: 640px;
    margin: 0 auto;
}
</style>
