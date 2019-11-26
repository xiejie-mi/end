<template>
<el-container style="height: 500px; border: 1px solid #eee">
    <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
        <el-menu :default-openeds="['1', '3']">
            <el-submenu index="1">
                <template slot="title"><i class="el-icon-message"></i>导航一</template>
                <el-menu-item-group>
                    <template slot="title">分组一</template>
                    <el-menu-item @click="Sadd">增加</el-menu-item>
                    <el-menu-item @click="Dsign">扣除</el-menu-item>
                </el-menu-item-group>

            </el-submenu>

        </el-menu>
    </el-aside>

    <el-container>
        <el-header style="text-align: right; font-size: 12px">
            <el-dropdown>
                <i class="el-icon-setting" style="margin-right: 15px"></i>
                <el-dropdown-menu slot="dropdown">
                    <el-row> <el-button type="info" icon="el-icon-message" circle @click="Message()"></el-button></el-row>
                    <el-row><el-button type="primary" icon="el-icon-edit" circle @click="Sign()"></el-button></el-row>
                    <el-row> <el-button type="danger" icon="el-icon-delete" circle @click="Exit()"></el-button></el-row>
                </el-dropdown-menu>
            </el-dropdown>
            <span>王小虎</span>
            <span>积分：120</span>
        </el-header>

        <el-main>
            <el-table :data="tableData">
                <el-table-column prop="created_time" label="日期" width="140">
                </el-table-column>
                <el-table-column prop="name" label="姓名" width="120">
                </el-table-column>
                <el-table-column prop="sort" label="总积分">
                </el-table-column>
                <el-table-column prop="desc" label="描述">
                </el-table-column>
            </el-table>
        </el-main>
    </el-container>
</el-container>
</template>
<style>
    .el-header {
        background-color: #B3C0D1;
        color: #333;
        line-height: 60px;
    }

    .el-aside {
        color: #333;
    }
</style>

<script>
    export default {
        data() {
            const item = {
                created_time: '2016-05-02',
                name: '王小虎',
                sort: '上海市普陀区金沙江路 1518 弄',
                desc:''
            };
            return {
                tableData: Array(20).fill(item)
            }
        },
        methods:{
            Sign(){//签到
                // 为给定 ID 的 user 创建请求
                var _this = this;//
                _this.$axios.get('/api/api/sign')//为后台交互
                    .then(function (response) {
                        console.log(response);
                        alert(response.data.msg);//弹出消息
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },
            Exit(){//退出
                // 为给定 ID 的 user 创建请求
                var _this = this;
                _this.$axios.get('/api/api/exituse')//为后台交互
                    .then(function (response) {
                        console.log(response);
                        alert(response.data.msg);//弹出消息
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },
            Message(){
                // 为给定 ID 的 user 创建请求
                this.$router.push('Message');
            },
            Sadd(){//查询增加的信息
                // 为给定 ID 的 user 创建请求
                var _this = this;
                _this.$axios.get('/api/api/sadd?type='+1)//弹出消息
                    .then(function (response) {
                        console.log(response);
                        _this.tableData=response.data.data;//替换数据
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },
           Dsign(){//查询减少的信息
                // 为给定 ID 的 user 创建请求
                var _this = this;
                _this.$axios.get('/api/api/sadd?type='+2)
                    .then(function (response) {
                        console.log(response);
                        _this.tableData=response.data.data;//替换数据
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },

        },
        created() {
            // 为给定 ID 的 user 创建请求
            var _this = this;
            _this.$axios.get('/api/api/useinfo')
                .then(function (response) {
                    console.log(response);
                 _this.tableData=response.data.data;
                })
                .catch(function (error) {
                    console.log(error);
                });
        }

    };
</script>