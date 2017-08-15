<template>
<div>
    <form :id="formId" :method="method" enctype="multipart/form-data">
        <input name="token" type="hidden" :value="token">
        <input name="file" :id="pickerId" type="file" @change="upload" :accept="accept" />
        <input name="accept" type="hidden" />
        <slot name="form">
        </slot>
    </form>

    <!-- add file -->
    <label :for="pickerId">
            <slot name="picker">
                <em>upload</em>
            </slot>
        </label>
</div>
</template>

<script>
export default {
    props: {
        method: {
            type: String,
            default: 'post'
        },
        action: {
            type: String,
            default: ''
        },
        token: {
            type: String,
            default: ''
        },
        accept: {
            type: String,
            default: 'image/png, image/jpeg, image/gif'
        },
        callback: {
            type: Function,
            default: () => {
                // console.log('upload done');
            }
        }
    },
    data: function () {
        return {
            key: '',
            formId: `upload${new Date().getTime()}`,
            pickerId: `picker${new Date().getTime()}`
        }
    },
    methods: {
        upload () {
            let that = this
            this.$nextTick(() => {
                let formData = new FormData(document.getElementById(this.formId))
                this.$http.post(this.action, formData, {
                    progress (e) {
                        if (e.lengthComputable) {
                            that.$emit('on-progress', e)
                        }
                    }
                }).then(res => {
                    this.$emit('on-upload', res.body)
                }, res => {
                    this.$emit('on-error', res.body)
                })
            })
        }
    }
}
</script>

<style scoped>
form {
    display: none;
}
</style>
