<template>
    <div class="header">
        <ul class="list">
            <a href="/article"><li>首页</li></a>
            <a href="/article/mind"><li>随笔</li></a>
            <a href="/article/project"><li>项目</li></a>
            <a href="/article/message"><li>留言</li></a>
            <a href="/article/about"><li>关于我</li></a>
            <a href="https://github.com/LegendRocky" target="_blank"><li>GitHub</li></a>
            <a href="/#/readme"><li>后台管理</li></a>
        </ul>
        <div class="user-info">
            <el-dropdown trigger="click" @command="handleCommand">
                <span class="el-dropdown-link">
                    <img class="user-logo" src="../../../../static/img/img.jpg">
                    {{username}}
                </span>
                <el-dropdown-menu slot="dropdown">
                    <el-dropdown-item command="loginout">退出</el-dropdown-item>
                </el-dropdown-menu>
            </el-dropdown>
        </div>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                name: 'linxin'
            }
        },
        computed:{
            username(){
                let username = localStorage.getItem('ms_username');
                return username ? username : this.name;
            }
        },
        methods:{
            handleCommand(command) {
                if(command == 'loginout'){
                    localStorage.removeItem('ms_username')
                    this.$router.push('/login');
                }
            }
        }
    }
</script>
<style scoped>
    .header {
        position: relative;
        box-sizing: border-box;
        width: 100%;
        height: 84px;
        font-size: 22px;
        line-height: 84px;
        color: #555;
        background: #fff;
    }
    .list{
        width: 70%;
        display: inline-block;
        font-size: 0px;
    }
    .list a{
        display: inline-block;
        text-align: center;
        width: 14%;
        color: #555;
        font-size: 16px;
        box-sizing: border-box;
        list-style: none;
    }
    .list a:hover{
        color: black;
    }
    .header .logo{
        float: left;
        width:250px;
        text-align: center;
    }
    .user-info {
        float: right;
        padding-right: 50px;
        font-size: 16px;
        color: #fff;
    }
    .user-info .el-dropdown-link{
        position: relative;
        display: inline-block;
        padding-left: 50px;
        color: #fff;
        cursor: pointer;
        vertical-align: middle;
    }
    .user-info .user-logo{
        position: absolute;
        left:0;
        top:15px;
        width:40px;
        height:40px;
        border-radius: 50%;
    }
    .el-dropdown-menu__item{
        text-align: center;
    }
</style>
