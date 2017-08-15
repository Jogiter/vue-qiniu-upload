<template>
    <div id="app">
        <upload :action="action" :token="token" @on-upload="uploadFile" :accept="accept" @on-error="uploadErr" @on-progress="progress">
            <template slot="form">
            </template>
            <template slot="picker">
                <span class="btn">上传图片</span>
            </template>
        </upload>
        <div class="block">
            <h2>上传结果</h2>
            <div id="result">
                <p>hash: {{imghash}}</p>
                <p>key: {{imgkey}}</p>
            </div>
            <img :src="imgHash" alt="imgHash" class="viewimg">
            <img :src="imgKey" alt="imgKey" class="viewimg">
        </div>
        <div class="block" id="ajaxErr" v-show="uploadMsg.length">
            <h4>uploadMsg: </h4>
            <p v-for="(item, index) in uploadMsg" :key="index">{{item}}</p>
        </div>
    </div>
</template>

<script>
    import upload from './components/upload.vue'
    export default {
        name: 'app',
        components: {
            upload
        },
        data: () => {
            return {
                action: 'http://upload.qiniu.com/', // 替换自己的上传链接
                accept: 'image/png, image/jpeg, image/gif',
                token: 'QWYn5TFQsLLU1pL5MFEmX3s5DmHdUThav9WyOWOm:JcGnWGIzc0POt2eEWWqUUsW8bN0=:eyJkZWxldGVBZnRlckRheXMiOjcsInNjb3BlIjoianNzZGsiLCJkZWFkbGluZSI6MTUwMjc5MTczMX0=', // 从[http://jssdk.demo.qiniu.io/formdata]获取
                imghash: '',
                imgkey: '',
                uploadMsg: []
            }
        },
        computed: {
            imgHash: function () {
                return `http://7j1xky.com2.z0.glb.qiniucdn.com/${this.imghash}`
            },
            imgKey: function () {
                return `http://7j1xky.com2.z0.glb.qiniucdn.com/${this.imgkey}`
            }
        },
        methods: {
            uploadFile (res) {
                this.imghash = res.hash
                this.imgkey = res.key
            },
            uploadErr (res) {
                this.uploadMsg.push(JSON.stringify(res))
            },
            progress (e) {
                this.uploadMsg.push(`e.loaded: ${e.loaded}, e.total: ${e.total}, percent: ${(e.loaded / e.total) * 100}`)
            }
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: #2c3e50;
        text-align: center;
        margin-top: 60px;
    }
    .block,
    .viewimg {
        margin: 15px;
    }
    .btn {
        padding: 5px 10px;
        border: 1px solid #ccc;
        border-radius: 3px;
    }
    .viewimg {
        max-width: 200px;
        border: 1px dashed #ccc;
        border-radius: 4px;
        padding: 5px;
        background-color: #ccc;
    }
</style>
