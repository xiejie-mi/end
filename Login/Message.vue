<template>
<el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
    <el-form-item label="用户姓名" prop="name">
        <el-input v-model="ruleForm.name" style="width: 45%"></el-input>
    </el-form-item>
    <el-form-item label="活动区域" >
        <el-cascader :options="options" clearable v-model="ruleForm.region"></el-cascader>

    </el-form-item>
    <el-form-item label="活动时间" required>

                <el-date-picker
                        v-model="ruleForm.date1"
                        type="date"
                        placeholder="选择日期">
                </el-date-picker>

    </el-form-item>

    <el-form-item label="电话" prop="desc">
        <el-input type="text" v-model="ruleForm.desc" style="width: 45%"></el-input>
    </el-form-item>
    <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
        <el-button @click="resetForm('ruleForm')">重置</el-button>
    </el-form-item>
</el-form>
</template>
<script>
    import {formatDate} from '../../../../date.js';

    export default {

           data() {
            return {
                ruleForm: {
                    name: '',
                    region: '',
                    date1: '',
                    desc: ''
                },


                options: [{
                    value: 'zhinan',
                    label: '指南',
                    children: [{
                        value: 'shejiyuanze',
                        label: '设计原则',
                        children: [{
                            value: 'yizhi',
                            label: '一致'
                        }]
                    }]
                }],
                rules: {
                    name: [
                        { required: true, message: '请输入活动名称', trigger: 'blur' },
                        { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
                    ],
                    region: [
                        { required: true, message: '请选择地区', trigger: 'change' }
                    ],
                    date1: [
                        { type: 'date', required: true, message: '请选择日期', trigger: 'change' }
                    ],
                    desc: [
                        { required: true, message: '请填写电话', trigger: 'blur' }
                    ]
                }
            };
        },
        methods: {
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        // 为给定 ID 的 user 创建请求
                        var _this = this;
                        var data = _this.ruleForm;

                        // 为给定 ID 的 user 创建请求
                        _this.$axios.post('/api/api/message',data)
                            .then(function (response) {
                                console.log(response);
                                if (response.data.code==0){
                                    alert('添加成功');

                                }

                            })
                            .catch(function (error) {
                                console.log(error);
                            });
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
            resetForm(formName) {
                this.$refs[formName].resetFields();
            },

        },
        created() {
            // 为给定 ID 的 user 创建请求
            var _this = this;
            // 为给定 ID 的 user 创建请求
            _this.$axios.post('/api/api/city')
                .then(function (response) {
                    console.log(response);
                    _this.options = response.data;


                })
                .catch(function (error) {
                    console.log(error);
                });
        }
    }
</script>