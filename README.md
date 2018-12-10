vue-qiniu-upload
================

> vue2.0 七牛图片上传，支持多张上传

install
-------

```sh
> npm install vue-resource --save
> npm install vue-qiniu-upload --save
```

import
------

```js
import vueUpload from 'vue-qiniu-upload'
...

Vue.component('vue-upload', vueUpload)
```

> note: 需要从[源码获取token](http://jssdk.demo.qiniu.io/formdata)

>上传照片后，显示时会翻转(关于img的orientation问题)，参见[图片基本处理](https://developer.qiniu.com/dora/manual/1279/basic-processing-images-imageview2):如果原图带有EXIF信息且包含Orientation字段，imageView2默认根据此字段的值进行自动旋转修正。 具备处理动态gif图片的能力

usage
-----

```vue
<template>
    <div id="app">
        <upload :action="action" :token="token" :multiple="multiple" :accept="accept" @on-upload="uploadFile" @on-error="uploadErr" @on-progress="progress">
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
                multiple: true,
                // 从[http://jssdk.demo.qiniu.io/formdata]源码获取token
                token: 'QWYn5TFQsLLU1pL5MFEmX3s5DmHdUThav9WyOWOm:JcGnWGIzc0POt2eEWWqUUsW8bN0=:eyJkZWxldGVBZnRlckRheXMiOjcsInNjb3BlIjoianNzZGsiLCJkZWFkbGluZSI6MTUwMjc5MTczMX0=',
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
```

特别感谢
--------

-	[js-sdk](https://github.com/qiniu/js-sdk/#概述) 基于七牛 API 开发的前端 JavaScript SDK
-	[Formdata 上传 demo](http://jssdk.demo.qiniu.io/formdata) Formdata 上传 demo
-	[表单上传，关键参数说明](https://developer.qiniu.com/kodo/manual/1272/form-upload)
