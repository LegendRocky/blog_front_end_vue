<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-date"></i> 文章管理</el-breadcrumb-item>
                <el-breadcrumb-item>写文章</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="plugins-tips">
            本项目使用的编辑器：<a href="https://github.com/surmon-china/vue-quill-editor" target="_blank">vue-quill-editor</a>
            ，此外再介绍一款国人开发的富文本编辑器：<a href="http://www.wangeditor.com/" target="_blank">wangEditor</a>
        </div>
        <div class="handle-box">
            <el-input v-model="title" placeholder="文章标题" class="handle-input mr10"></el-input>
        </div>
        <div class="handle-box">
            <el-input v-model="subtitle" type="textarea" placeholder="文章简介"></el-input>
        </div>
        <quill-editor ref="myTextEditor" v-model="content" :config="editorOption"></quill-editor>
        <el-button class="editor-btn" type="primary" @click="submit">提交</el-button>
    </div>
</template>

<script>
    import { quillEditor } from 'vue-quill-editor';
    export default {
        data: function(){
            return {
                geturl: '/article/:id/getArticle',
                updateurl: '/article/:id/updateArticle',
                content: '',
                title: '',
                subtitle: '',
                editorOption: {
                    // something config
                }
            }
        },
        created(){
            this.getData();

        },
        components: {
            quillEditor
        },
        methods: {
            onEditorChange({ editor, html, text }) {
                this.content = html;
            },
            submit(){
                let self = this;
                let id = this.$route.query.id;
                let method = "put";
                let url = self.updateurl.replace(/:id/g, id);
                console.log(self.title);
                if (!id) {
                    method = "post";
                    url = '/article/addArticle';
                }
                self.axios({
                    method: method,
                    url: url,
                    data: {
                        title: self.title,
                        subtitle: self.subtitle,
                        content: self.content
                    }
                }).then((res) => {
                    console.log(res.data);
                    this.$message.success('提交成功！');
                })
            },
            getData() {
                let self = this;
                let id = this.$route.query.id;
                if ( !id ) {
                    return self.content = '';
                }
                self.axios({
                    method: "get",
                    url: self.geturl.replace(/:id/g, id)
                }).then((res) => {
                    if ( res.data.data.content ) {
                        console.log(res.data.data);
                        self.content = res.data.data.content;
                        self.title = res.data.data.title;
                        self.subtitle = res.data.data.subtitle;
                    }
                })
            }
        },
        computed: {
            editor() {
                return this.$refs.myTextEditor.quillEditor;
            }
        }
    }
</script>
<style scoped>
    .editor-btn{
        margin-top: 20px;
    }
    .handle-box{
        margin-bottom: 20px;
    }
    .handle-select{
        width: 120px;
    }
    .handle-input{
        width: 300px;
        display: inline-block;
    }
</style>