vue-qiniu-upload
================

> 多级联动地址选择器

install
-------

```sh
> npm install vue-qiniu-upload --save
> npm install vue-resource --save
```

import
------

```js
import vueUpload from 'vue-qiniu-upload'
...

Vue.component('vue-upload', vueUpload)
```

usage
-----

```vue
<template>
    <div id="app">
        <upload :action="action" :token="token" @on-upload="uploadFile" :accept="accept">
            <p slot="picker" class="upload-btn">
                <span>上传图片</span>
            </p>
        </upload>
        <div class="block">
            <h2>上传结果</h2>
            <div id="result">
                <p>hash: {{hash}}</p>
                <p>key: {{key}}</p>
            </div>
            <img :src="imgHash" alt="" class="viewimg">
            <img :src="imgKey" alt="" class="viewimg">
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
                accept: 'image/*',
                token: 'QWYn5TFQsLLU1pL5MFEmX3s5DmHdUThav9WyOWOm:Mfqr60SDX7O1eK_yjojyXQCbnbU=:eyJkZWxldGVBZnRlckRheXMiOjcsInNjb3BlIjoianNzZGsiLCJkZWFkbGluZSI6MTUwMjcwMTAyOH0=', // 从[http://jssdk.demo.qiniu.io/formdata]获取
                hash: '',
                key: ''
            }
        },
        computed: {
            imgHash: function () {
                return `http://7j1xky.com2.z0.glb.qiniucdn.com/${this.hash}`
            },
            imgKey: function () {
                return `http://7j1xky.com2.z0.glb.qiniucdn.com/${this.key}`
            }
        },
        methods: {
            uploadFile (res) {
                this.hash = res.hash
                this.key = res.key
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
    }
</style>
```

特别感谢
--------

-	[js-sdk](https://github.com/qiniu/js-sdk/#概述) 基于七牛 API 开发的前端 JavaScript SDK
-	[Formdata 上传 demo](http://jssdk.demo.qiniu.io/formdata) Formdata 上传 demo
-	[表单上传，关键参数说明](https://developer.qiniu.com/kodo/manual/1272/form-upload)
