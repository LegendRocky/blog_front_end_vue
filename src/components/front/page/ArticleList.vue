<template>
    <div class="content-container">
        <ul>
            <li v-for="item in articles">
                <div class="item-right">
                    <p class="title"><a :href="'/#/articledetail?id='+item._id">{{ item.title }}</a></p>
                    <p class="time">{{ item.publish_time }}</p>
                    <p class="subtitle">{{ item.subtitle }}</p>
                </div>  
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                url: '/article/getArticleList',
                articles: [],
                cur_page: 1,
                multipleSelection: [],
                select_cate: '',
                select_word: '',
                del_list: [],
                is_search: false,
                total_count: 100
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
                // if(process.env.NODE_ENV === 'development'){
                //     self.url = '/ms/table/list';
                // };
                self.axios({
                    method: "get",
                    url: self.url,
                    params: {
                      currentPage: this.cur_page
                    }
                }).then((res) => {
                    console.log(res.data);
                    if(res.data.count) this.total_count = res.data.count;
                    self.articles = res.data.data;
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
            },
            handleDelete(index, row) {
                this.$message.error('删除第'+(index+1)+'行');
            },
            delAll(){
                const self = this,
                    length = self.multipleSelection.length;
                let str = '';
                self.del_list = self.del_list.concat(self.multipleSelection);
                for (let i = 0; i < length; i++) {
                    str += self.multipleSelection[i].name + ' ';
                }
                self.$message.error('删除了'+str);
                self.multipleSelection = [];
            },
            handleSelectionChange(val) {
                this.multipleSelection = val;
            }
        }
    }
</script>

<style scoped>
    ul li{
        list-style: none;
        background: #fff;
        padding: 20px;
        margin-bottom: 20px;
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
    p.subtitle{
        width: 100%;
        height: auto;
        white-space: normal;
        word-break: break-all;
        overflow-x: hidden;
        line-height: 22px;
        color: #666;
        font-size: 15px;
    }
</style>