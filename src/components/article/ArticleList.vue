<template>
    <div class="table">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-menu"></i> 文章管理</el-breadcrumb-item>
                <el-breadcrumb-item>文章列表</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="handle-box">
            <el-button type="primary" icon="delete" class="handle-del mr10" @click="delAll">批量删除</el-button>
            <el-input v-model="select_word" placeholder="筛选关键词" class="handle-input mr10"></el-input>
            <el-button type="primary" icon="search" @click="search">搜索</el-button>
        </div>
        <el-table :data="data" border style="width: 100%" ref="multipleTable" @selection-change="handleSelectionChange">
            <el-table-column type="selection" width="55"></el-table-column>
            <el-table-column prop="title" label="标题" sortable width="150"></el-table-column>
            <el-table-column prop="tags" label="标签" :formatter="formatter"></el-table-column>
            <el-table-column prop="publish_time" label="创建日期" width="220"></el-table-column>
            <el-table-column prop="updated" label="更新日期" width="220"></el-table-column>
            <el-table-column label="操作" width="220">
                <template scope="scope">
                    <el-button size="small"
                            @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                    <el-button size="small" type="danger"
                            @click="handleDelete(scope.$index, scope.row)">删除</el-button>
                    <el-button size="small" type="primary"
                            @click="handleView(scope.$index, scope.row)">预览</el-button>
                </template>
            </el-table-column>
        </el-table>
        <div class="pagination">
            <el-pagination
                    @current-change ="handleCurrentChange"
                    layout="prev, pager, next"
                    :total="total_count">
            </el-pagination>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                url: '/article/getArticleList',
                deleteurl: '/article',
                tableData: [],
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
                return self.tableData.filter(function(d){
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
            handleCurrentChange(val){
                this.cur_page = val;
                this.getData();
            },
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
                    self.tableData = res.data.data;
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
                let id = this.data[index]._id;
                let self = this;
                self.axios({
                    method: "delete",
                    url: self.deleteurl,
                    data: {
                        id: id
                    }
                }).then((res) => {
                    console.log(res.data);
                    this.$message.success('删除成功！');
                    self.getData();
                })
            },
            handleView(index, row) {
                this.$router.push('/articledetail?id=' + this.data[index]._id);
            },
            delAll(){
                const self = this,
                    length = self.multipleSelection.length;
                let str = '';
                let del_ids = [];
                for (let i = 0; i < length; i++) {
                    if(self.multipleSelection[i]._id) del_ids.push(self.multipleSelection[i]._id);
                }
                console.log(del_ids);
                self.axios({
                    method: "delete",
                    url: self.deleteurl,
                    data: {
                        id: del_ids
                    }
                }).then((res) => {
                    console.log(res.data);
                    this.$message.success('删除成功！');
                    self.getData();
                    self.multipleSelection = [];
                })
            },
            handleSelectionChange(val) {
                this.multipleSelection = val;
            }
        }
    }
</script>

<style scoped>
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