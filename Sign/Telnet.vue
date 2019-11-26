<template>
<el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
<el-form-item label="用户名称" prop="name">
    <el-input v-model="ruleForm.name"></el-input>
</el-form-item>
<el-form-item label="用户密码" prop="desc">
    <el-input type="password" v-model="ruleForm.desc"></el-input>
</el-form-item>
<el-form-item>
    <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
    <el-button @click="resetForm('ruleForm')">重置</el-button>
</el-form-item>
</el-form>
</template>
<script>
    export default {
        data() {
            return {
                ruleForm: {
                    name: '',
                    desc: ''
                },
                rules: {//表单验证
                    name: [
                        { required: true, message: '请输入用户名称', trigger: 'blur' },
                        { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
                    ],
                    desc: [
                        { required: true, message: '请填写密码', trigger: 'blur' }
                    ]
                }
            };
        },
        methods: {
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        var _this=this;
                        console.log(this.ruleForm);
                        var data = this.ruleForm;
                        // 为给定 ID 的 user 创建请求
                        _this.$axios.post('/api/api/telnet',data)//把数据传到前台去
                            .then(function (response) {
                                console.log(response);
                                if (response.data.code==0){//判断是否登录成功
                                    alert('登录成功');
                                    _this.$router.push('Show');
                                }else{
                                    alert('登录失败');
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
            }
        }
    }
</script>
