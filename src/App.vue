<template>
    <div id="app">
        <upload :action="action" :token="token" @on-upload="uploadFile" :accept="accept" :on-e="uploadErr">
            <template slot="form">
                <input name="key" type="hidden" :value="key">
            </template>
            <p slot="picker" class="upload-btn">
                <span>上传图片</span>
            </p>
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
        <div class="block" id="ajaxErr"></div>
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
                accept: 'image/*',
                key: Math.floor(Math.random() * 100) + Math.random().toString(32).slice(8),
                token: 'QWYn5TFQsLLU1pL5MFEmX3s5DmHdUThav9WyOWOm:Mfqr60SDX7O1eK_yjojyXQCbnbU=:eyJkZWxldGVBZnRlckRheXMiOjcsInNjb3BlIjoianNzZGsiLCJkZWFkbGluZSI6MTUwMjcwMTAyOH0=', // 从[http://jssdk.demo.qiniu.io/formdata]获取
                imghash: '',
                imgkey: ''
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
                console.log(res)
                alert(res)
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
    .viewimg {
        max-width: 200px;
        border: 1px dashed #ccc;
        border-radius: 4px;
        padding: 5px;
        background-color: #ccc;
    }
</style>
