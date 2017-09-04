<template>
    <div class="content-container">
        <div class="article-detail">
            <p class="title">{{ title }}</p>
            <p class="time">{{ publish_time }}</p>
            <p class="subtitle">{{ subtitle }}</p>
            <p class="item-content" v-html="content"></p>
        </div>  
    </div>
</template>

<script>
    export default {
        data() {
            return {
                geturl: '/article/:id/getArticle',
                title: '',
                subtitle: '',
                content: '',
            }
        },
        created(){
            this.getData();
        },
        computed: {
            data(){
                const self = this;
                return self.articles.filter(function(d){
                    let is_del = false;
                    for (let i = 0; i < self.del_list.length; i++) {
                        if(d.name === self.del_list[i].name){
                            is_del = true;
                            break;
                        }
                    }
                    if(!is_del){
                        // if(d.address.indexOf(self.select_cate) > -1 && 
                        //     (d.name.indexOf(self.select_word) > -1 ||
                        //     d.address.indexOf(self.select_word) > -1)
                        // ){
                            return d;
                        //}
                    }
                })
            }
        },
        methods: {
            getData(){
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
                        self.publish_time = res.data.data.publish_time;
                    }
                })
            },
            search(){
                this.is_search = true;
            },
            formatter(row, column) {
                return row.address;
            },
            filterTag(value, row) {
                return row.tag === value;
            },
            handleEdit(index, row) {
                console.log(this.data[index]);
                this.$router.push('/articleedit?id='+this.data[index]._id);
            }
        }
    }
</script>

<style scoped>
    .article-detail{
        background: #fff;
        padding: 20px;
        border-radius: 6px;
    }
    .content-container{
        padding: 40px 20px;
        margin: 0 auto;
        max-width: 1130px;
    }
    p.title{
        line-height: 26px;
        font-size: 18px;
        height: 26px;
        width: 100%;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;  
    }
    p.title a{
        color: #333;
    }
    p.time{
        color: #666;
        font-size: 14px;
        margin: 10px 0;
    }
    p.subtitle, .item-content{
        width: 100%;
        height: auto;
        white-space: normal;
        word-break: break-all;
        overflow-x: hidden;
        line-height: 22px;
        color: #666;
        font-size: 15px;
        margin-bottom: 40px;
    }

</style>