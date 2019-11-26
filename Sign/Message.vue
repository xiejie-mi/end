<template>
<el-form :model="numberValidateForm" ref="numberValidateForm" label-width="100px" class="demo-ruleForm">
    <el-form-item
            label="电话"
            prop="age"
            :rules="[
      { required: true, message: '电话不能为空'},
      { type: 'number', message: '电话必须为数字值'}
    ]"
    >
        <el-input type="age" v-model.number="numberValidateForm.age" autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item>
        <el-button type="primary" @click="submitForm('numberValidateForm')">提交</el-button>
        <el-button @click="resetForm('numberValidateForm')">重置</el-button>
    </el-form-item>
</el-form>
</template>
<script>
    export default {
        data() {
            return {
                numberValidateForm: {
                    age: ''
                }
            };
        },
        methods: {
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        var _this=this;
                        console.log(this.numberValidateForm);
                        var data = this.numberValidateForm;
                        // 为给定 ID 的 user 创建请求
                        _this.$axios.post('/api/api/message',data)//数据传到前台
                            .then(function (response) {
                                console.log(response);
                                if (response.data.code==0){
                                    alert('完善成功');
                                    _this.$router.push("Show");
                                }else{
                                    alert('完善失败');
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