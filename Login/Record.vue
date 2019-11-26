<template>
<el-container style="height: 500px; border: 1px solid #eee">
    <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
        <el-menu :default-openeds="['1', '3']">
            <el-submenu index="1">
                <template slot="title"><i class="el-icon-message"></i>导航一</template>
                <el-menu-item-group>
                    <el-menu-item @click="inc()">增加积分</el-menu-item>
                    <el-menu-item @click="der()">消耗积分</el-menu-item>
                </el-menu-item-group>

            </el-submenu>

        </el-menu>
    </el-aside>

    <el-container>
        <el-header style="text-align: right; font-size: 12px">
            <el-dropdown>
                <i class="el-icon-setting" style="margin-right: 15px"></i>
                <el-dropdown-menu slot="dropdown">
                    <el-dropdown-item>查看</el-dropdown-item>
                    <el-dropdown-item>新增</el-dropdown-item>
                    <el-dropdown-item>删除</el-dropdown-item>
                </el-dropdown-menu>
            </el-dropdown>
            <span>王小虎</span>
        </el-header>

        <el-main>
            <el-table :data="tableData">
                <el-table-column prop="updated_at" label="日期" width="140">
                </el-table-column>
                <el-table-column prop="desc" label="详细信息" width="120">
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
                updated_at: '2016-05-02',
                desc: '王小虎',

            };
            return {
                tableData: Array(2).fill(item)
            }
        },

        created() {
            // 为给定 ID 的 user 创建请求
            var _this = this;
            // 为给定 ID 的 user 创建请求
            _this.$axios.post('/api/api/details')
                .then(function (response) {
                    console.log(response);
                    _this.tableData=response.data.data;


                })
                .catch(function (error) {
                    console.log(error);
                });
        },
        methods:{
            inc(){
                // 为给定 ID 的 user 创建请求
                var _this = this;
                // 为给定 ID 的 user 创建请求
                _this.$axios.post('/api/api/detailsadd?type='+1)
                    .then(function (response) {
                        console.log(response);
                        _this.tableData=response.data.data;


                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },
            der(){
                // 为给定 ID 的 user 创建请求
                var _this = this;
                // 为给定 ID 的 user 创建请求
                _this.$axios.post('/api/api/detailsdre?type='+0)
                    .then(function (response) {
                        console.log(response);
                        _this.tableData=response.data.data;


                    })
                    .catch(function (error) {
                        console.log(error);
                    });

            }

        }
    };
</script>