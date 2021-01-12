<template>
  <div class="maxbox">
    <div id="appp">
      <vue-particles
        color="#dedede"
        :particle-opacity="0.7"
        :particles-number="80"
        shape-type="circle"
        :particle-size="4"
        lines-color="#dedede"
        :lines-width="1"
        :line-linked="true"
        :line-opacity="0.4"
        :lines-distance="150"
        :move-speed="3"
        :hover-effect="true"
        hover-mode="grab"
        :click-effect="true"
        click-mode="push"
        class="beijing"
      />
    </div>
    <!--
下面介绍参数配置文件 app.js 文件的使用：
color: String类型。默认'#dedede'。粒子颜色。
particleOpacity: Number类型。默认0.7。粒子透明度。
particlesNumber: Number类型。默认80。粒子数量。
shapeType: String类型。默认'circle'。可用的粒子外观类型有："circle","edge","triangle", "polygon","star"。
particleSize: Number类型。默认80。单个粒子大小。
linesColor: String类型。默认'#dedede'。线条颜色。
linesWidth: Number类型。默认1。线条宽度。
lineLinked: 布尔类型。默认true。连接线是否可用。
lineOpacity: Number类型。默认0.4。线条透明度。
linesDistance: Number类型。默认150。线条距离。
moveSpeed: Number类型。默认3。粒子运动速度。
hoverEffect: 布尔类型。默认true。是否有hover特效。
hoverMode: String类型。默认true。可用的hover模式有: "grab", "repulse", "bubble"。
clickEffect: 布尔类型。默认true。是否有click特效。
clickMode: String类型。默认true。可用的click模式有: "push", "remove", "repulse", "bubble"
class="beijing" 设置背景图片
<style lang="less">
// 这一个部分是插件的哦
.beijing {
background-image: url("./start.jpg");
background-size: cover;
position: absolute;
top: 0;
left: 0;
width: 100%;
height: 100%;
}
</style>
-->
    <el-form
      ref="loginForm"
      :model="loginForm"
      status-icon
      :rules="rules"
      label-width="100px"
      class="demo-ruleForm"
    >
      <h1 class="title">
        <i class="el-icon-menu" />
        华联超市管理系统-登录
      </h1>
      <el-form-item label="账号" prop="username">
        <el-input v-model="loginForm.username" type="text" />
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input v-model="loginForm.password" type="text" autocomplete="off" />
      </el-form-item>
      <el-form-item label="确认密码" prop="checkPass">
        <el-input v-model="loginForm.checkPass" type="text" autocomplete="off" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('loginForm')">提交</el-button>
        <el-button @click="resetForm('loginForm')">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
export default {
  data() {
    // 包含特殊字符哦
    const checkSpecificKey = function(str) {
      // 正则表达
      const specialKey = "[`~!#$^&*()=|{}':;',\\[\\].<>/?~！#￥……&*（）——|{}【】‘；：”“'。，、？]‘'"
      for (let i = 0; i < str.length; i++) {
        if (specialKey.indexOf(str.substr(i, 1)) !== -1) {
          // specialKey是否含有某一个你输入的特殊字符
          return false // 如果你输入的账号中有特殊字符 直接退出来哦
        }
      }
      return true
    }
    // 验证密码的函数
    const pass = (rule, value, callback) => {
      if (value === '') {
        // 为空的验证
        callback(new Error('请输入密码'))
      } else if (value.length < 3 || value.length > 6) {
        // 这是密码长度的验证
        callback(new Error('密码长度 3 - 6 位'))
      } else if (!checkSpecificKey(value)) {
        //
        callback(new Error('密码不能包含特殊字符'))
      } else {
        if (this.loginForm.checkPass !== '') {
          // 如果确认密码不为空
          this.$refs.loginForm.validateField('checkPass') // 调用确认密码的验证（一致性验证）
        }
        // 成功的回调
        callback()
      }
    }
    // 确认密码的验证函数
    const checkPass = (rule, value, callback) => {
      // rule是传入的验证规则 value是用户输入的值 callback是一个回调函数
      if (value === '') {
        // 如果等于空
        console.log(value)
        callback(new Error('请再次输入密码11')) // 输出提示
      } else if (value !== this.loginForm.password) {
        // 如果确认密码 和 密码不同
        callback(new Error('两次输入密码不一致1'))
      }
      // 如果直接调用 不传入任何错误信息 就是成功 绿色的勾勾
      callback()
    }
    return {
      // 登录表单的数据
      loginForm: {
        username: '',
        password: '',
        checkPass: ''
      },
      // 验证的规则（一份数据）
      rules: {
        // 验证用户名
        username: [
          // 非空验证
          { required: true, message: '请输入账号', trigger: 'blur' },
          { min: 3, max: 5, message: '账号长度在 3 - 5 位', trigger: 'blur' }
        ],
        // 验证密码
        password: [
          // 非空验证
          { required: true, validator: pass, trigger: 'blur' }
        ],
        // 验证确认密码
        checkPass: [
          // 自定义验证函数
          { required: true, validator: checkPass, trigger: 'blur' }
        ]
        /*
验证规则字段说明：
{ required: true/false 必填, message: "错误的提示信息", trigger: "触发验证的方式" }
{ min: 最小长度, max: 最大长度, message: "错误的提示信息", trigger: "触发验证的方式" }
{ validator： 自定义验证规则函数名， trigger: 'blur'}
*/
      }
    }
  },
  methods: {
    // 点击登录按钮 触发这个函数
    submitForm(formName) {
      // 获取表单组件 调用验证方法
      this.$refs[formName].validate(valid => {
        // 如果所有验证通过 valid就是true
        if (valid) {
          alert('前端验证通过 可以提交给后端！')
          // 后续就可以把收集的账号和密码 一起发送给后端 验证用户名和密码的正确性。
          // 收集账号和密码
          const params = {
            username: this.loginForm.username,
            password: this.loginForm.password
          }
          // 发送请求 把参数发给后端（把用户名和密码发给后端 验证是否正确）
          // console.log(params)
          // 直接跳转到后端主页
          // this.$router.push("/");
        } else {
          // 否则就是false
          alert('前端验证失败 不能提交给后端！')
          return false
        }
      })
    },
    // 点击重置按钮 触发这个函数
    resetForm(formName) {
      // this.$refs.loginForm.resetFields() 获取整个表单组件 调用重置方法
      this.$refs[formName].resetFields()
    }
  }
}
</script>
<style lang="less">
  // 这一个部分是插件的哦
  .beijing {
  // background-image: url("./start.jpg");
  background-color:#5f9ea0;
  // background-size: cover;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  }
  #app {
    //最大的容器
    width: 100%;
    height: 100%;
    margin: 10 auto;
    text-align: center;
    .el-form {
    //第二大的容器
    // 垂直 水平 居中
    width: 450px;
    height: 350px;
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    margin: auto;
    .el-form-item {
    //这是那三行的错误哦
    margin-top: 28px; //让变淡之间是有间距的哦
    .el-form-item__label {
    color: #ffffff;
    }
    }
    .title {
    //超市管理系统的标题
    color: #ffffff;
    padding-left: 40px; //让变体居中 这里使用的是padding-left
    }
    }
  }
</style>
