<template>
<el-card header="上传图片" shadow="never" style="max-width: 1250px;margin: 20px auto;">
    <ele-form :form-data="formData" :form-desc="formDesc" :request-fn="handleRequest" @request-success="handleSuccess" />
    <el-card class="box-card">
        <div v-for="o in 1" :key="o" class="text item">
            URL : <el-link type="primary">{{url}}</el-link>
        </div>
    </el-card>

</el-card>
</template>

<script>
export default {
    name: 'App',
    data() {
        return {
            url: '',
            formData: {},
            formDesc: {
                covers: {
                    label: '封面',
                    type: 'image-uploader',
                    attrs: {
                        drag: true, // 多张
                        action: 'http://api.yanxiaolong.cn/qiniu/uploadImg',
                        responseFn(response, file) {
                            return 'http://image.yanxiaolong.cn/' + response.data

                        }
                    }
                }
            }
        }
    },
    methods: {
        handleRequest(data) {
            console.log(data)
            this.url = data.covers
            return Promise.resolve()
        },
        handleSuccess(response) {
            this.$message.success('提交成功')
        }
    },
    mounted() {}
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
