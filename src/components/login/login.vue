<template>
  <div class="login-wrap">
    <el-form class="login-form" label-position="top" label-width="80px" :model="formdata">
      <h2>用户登录</h2>
      <el-form-item label="用户名">
        <el-input v-model="formdata.username"></el-input>
      </el-form-item>
      <el-form-item label="密码">
        <el-input v-model="formdata.password"></el-input>
      </el-form-item>
      <!-- 按钮 -->
      <el-button @click.prevent="handleLogin()" class="login-btn" type="primary">登录</el-button>
    </el-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      formdata: {
        username: "",
        password: ""
      }
    };
  },

  methods: {
    // async 简化
    async handleLogin() {
      const res = await this.$http.post("login", this.formdata);
      console.log(res);
      const {
        meta: { status, msg },
        data
      } = res.data;
      if (status === 200) {
        // 设置请求头
        // localStorage.setItem : 将value存储到key字段
        const token = localStorage.setItem('token',data.token)
        // 编程式导航
        this.$router.push({ name: "home" });
        this.$message.success(msg);
      } else {
        // 错误提示框
        this.$message.warning(msg);
      }
    }

    // // 点击登录跳转的方法
    // handleLogin() {
    //   // // 接口数据
    //   this.$http.post('login',this.formdata)
    //   // !!! => 中间不加空格!!!
    //   .then((res)=>{
    //     console.log(res)
    //     // 解构赋值
    //     const {meta: {status,msg},data} = res.data
    //     // 判断状态成功时 跳转到home页
    //     if(status === 200) {
    //       // 编程式导航
    //       this.$router.push({name: 'home'})
    //       this.$message.success(msg)
    //     } else {
    //       // 错误提示框
    //       this.$message.warning(msg)
    //     }
    //   })
    // }
  }
};
</script>

<style>
.login-wrap {
  height: 100%;
  background-color: rgb(78, 80, 95);
  /* 伸缩盒子 */
  display: flex;
  justify-content: center;
  align-items: center;
}

.login-wrap .login-form {
  width: 400px;
  background-color: rgb(255, 255, 250);
  border-radius: 5px;
  padding: 30px;
}

.login-form h2 {
  margin: 0;
  padding: 0;
  margin-bottom: 10px;
}
.login-form .login-btn {
  width: 100%;
}
</style>
