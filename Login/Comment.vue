<template>
    <!-- 多文件上传存于本地文件 -->
    <!-- name="swe"//默认值为file -->

    <el-upload
            class="upload-demo"
            ref="upload"
            action="/api/api/image"
            name="swe"
            :on-preview="handlePreview"
            :on-remove="handleRemove"
            :on-success="btsuccess"
            :before-remove="beforeRemove"
            :on-change="btnchange"
            multiple
            :limit="3"
            :on-exceed="handleExceed"
            :file-list="fileList"
            :auto-upload="false">
        <!-- :auto-upload="false"禁止自动提交 -->
        <!-- slot="trigger" 避免点击上传到服务器打开上传文件界面 -->

        <el-button slot="trigger"size="small" type="primary" >点击上传</el-button>
        <el-button style="margin-left: 10px;" size="small" type="success" @click="submitUpload">上传到服务器</el-button>
        <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
    </el-upload><!-- multiple是否支持多选文件-->



</template>
<script>
    export default {
        'name':'Property',
        data() {
            return {
                fileList:[]
                // 一次提交不能有重复文件
            };
        },
        methods: {


            submitUpload() {
                this.$refs.upload.submit();
            },
            btnchange:function(file,fileList){
                console.log(fileList)
                for(var i=0;i<fileList.length;i++)
                {
                    for(var j=i+1;j<fileList.length;j++)
                    {
                        if(fileList[i]['name']===fileList[j]['name'])
                        {
                            this.$message.warning('该文件以选中');
                            fileList.pop(j)
                        }
                    }
                }
            },
            btsuccess:function(response,file,fileList){
                var _this=this;
                console.log(response)
                // if (response.code==0){
                //     _this.$router.push({name:'Exhibition'})
                // }

            },
            handleRemove(file, fileList) {
                console.log(file, fileList);

            },
            handlePreview(file) {
                console.log(file)

            },
            handleExceed(files, fileList) {
                this.$message.warning(`当前限制选择 3 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`);
            },
            beforeRemove(file, fileList) {
                return this.$confirm(`确定移除 ${ file.name }？`);
            }
        }
    }
</script>
