<template>
  <div class="index_container">
    <div class="homepage-hero-module">
      <div class="video-container">
        <div :style="fixStyle" class="filter"></div>
        <video :style="fixStyle" autoplay loop class="fillWidth" v-on:canplay="canplay">
          <source src="111.mp4" type="video/mp4" />浏览器不支持 video 标签，建议升级浏览器。
          <source src="PATH_TO_WEBM" type="video/webm" />浏览器不支持 video 标签，建议升级浏览器。
        </video>
        <div class="poster hidden" v-if="!vedioCanPlay">
          <img :style="fixStyle" src="PATH_TO_JPEG" alt />
        </div>
      </div>
    </div>
    <div class="index_form">
      <el-form
        :model="ruleForm"
        status-icon
        :rules="rules"
        ref="ruleForm"
        label-width="70px"
        class="demo-ruleForm"
        label-position="right"
      >
        <el-form-item label="用户名" prop="name">
          <el-input v-model="ruleForm.name"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="pass">
          <el-input type="password" v-model="ruleForm.pass" autocomplete="off"></el-input>
        </el-form-item>
        
        <el-form-item>
          <el-button type="primary" @click="submitForm('ruleForm')" style="width:140px;">登录</el-button>
           <el-button @click="submitEnroll" style="width:140px;">注册</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: "login",
  data() {
    var validatePass = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请输入密码"));
      } else {
        if (this.ruleForm.checkPass !== "") {
          this.$refs.ruleForm.validateField("checkPass");
        }
        callback();
      }
    };
    return {
      vedioCanPlay: false,
      fixStyle: "",
      ruleForm: {
        pass: "",
        name: ""
      },
      rules: {
        name: [
          { required: true, message: "请输入活动名称", trigger: "blur" },
          { min: 3, max: 5, message: "长度在 3 到 5 个字符", trigger: "blur" }
        ],
        pass: [{required: true, validator: validatePass, trigger: "blur" }],
       
      }
    };
  },
  methods: {
    canplay() {
      this.vedioCanPlay = true;
    },
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          // 为给定 ID 的 user 创建请求
          var _this = this;
          var data = _this.ruleForm;

          // 为给定 ID 的 user 创建请求
          _this.$axios.post('/api/api/integrallogin',data)
                  .then(function (response) {
                    console.log(response);
                    if (response.data.code==0){
                      alert('登录成功');
                      _this.$router.push('Comment')
                    }

                  })
                  .catch(function (error) {
                    console.log(error);
                  });

        } else {
          alert("请完成登录信息");
          return false;
        }
      });
    },
    submitEnroll(){
          this.$router.push('/enroll')

    }
  },
  mounted: function() {
    window.onresize = () => {
      const windowWidth = document.body.clientWidth;
      const windowHeight = document.body.clientHeight;
      const windowAspectRatio = windowHeight / windowWidth;
      let videoWidth;
      let videoHeight;
      if (windowAspectRatio < 0.5625) {
        videoWidth = windowWidth;
        videoHeight = videoWidth * 0.5625;
        this.fixStyle = {
          height: windowWidth * 0.5625 + "px",
          width: windowWidth + "px",
          "margin-bottom": (windowHeight - videoHeight) / 2 + "px",
          "margin-left": "initial"
        };
      } else {
        videoHeight = windowHeight;
        videoWidth = videoHeight / 0.5625;
        this.fixStyle = {
          height: windowHeight + "px",
          width: windowHeight / 0.5625 + "px",
          "margin-left": (windowWidth - videoWidth) / 2 + "px",
          "margin-bottom": "initial"
        };
      }
    };
    window.onresize();
  }
};
</script>

<style lang="less" scoped >
.index_container {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
  background: red;
}
.homepage-hero-module,
.video-container {
  position: relative;
  height: 100vh;
  overflow: hidden;
}

.video-container .poster img,
.video-container video {
  z-index: 0;
  position: absolute;
}

.video-container .filter {
  z-index: 1;
  position: absolute;
  background: rgba(0, 0, 0, 0.4);
}
.index_form {
   width: 400px;
  height: 200px;
  position: fixed;
  top: 50%;
  left: 50%;
  margin-left: -200px;
  margin-top: -100px;
  z-index: 2;
  padding: 20px 20px;
  box-sizing: border-box;
  background: #fff;
  border-radius: 10px;
}
</style>