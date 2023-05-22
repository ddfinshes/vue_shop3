<template>
    <div class="login_container">
        <div class="login_box">
            <!-- 头像区域 -->
            <div class="avatar-box">
                <img src="../assets/logo.png" alt="">
            </div>
            <!-- 登录表单区域 -->
            <el-form ref="loginFromRef" :model="loginForm" :rules="loginFormRules" label-width="0px" class="login_form">
                <!-- 账号 -->
                <el-form-item prop="username">
                    <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
                </el-form-item>
                <!-- 密码 -->
                <el-form-item prop="password">
                    <el-input v-model="loginForm.password" prefix-icon="iconfont icon-3702mima" type="password"></el-input>
                </el-form-item>
                <!-- 按钮区域 -->
                <el-form-item class="btns">
                    <el-button type="primary" @click="login">登录</el-button>
                    <el-button type="info" @click="resetLoginForm">重置</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            // 这是登录表单的数据绑定对象
            loginForm: {
                username: 'admin',
                password: '123456'
            },
            // 这是表单的验证规则对象
            loginFormRules: {
                // 验证用户名是否合法
                username: [
                    { required: true, message: '请输入登录名称', trigger: 'blur' },
                    { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
                ],
                // 验证密码是否合法
                password: [
                    { required: true, message: '请输入登录密码', trigger: 'blur' },
                    { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
                ]
            }
        }
    },
    methods: {
        // 点击重置按钮，重置登录表单
        resetLoginForm() {
            // console.log(this)
            this.$refs.loginFromRef.resetFields();
        },
        login() {
            // 表单预验证
            this.$refs.loginFromRef.validate(async (valid) => {
                if (!valid) return;
                // 将data匿名为res
                const { data: res } = await this.$http.post('login', this.loginForm);
                console.log(res.meta.status);
                if (res.meta.status === 200) {
                    this.$message.success("登录成功");
                    // 1. 将登录成功之后的 token， 保存到客户端的 sessionStorge中
                    //  1.1 项目中出了登录之外的其他API接口，必须在登录之后才能访问
                    //  1.2 token只应在当前网站打开期间生效，所以将token保存在sessionStorage中
                    console.log(res)
                    window.sessionStorage.setItem('token', res.data.token);
                    // 2. 通过编程导航跳转到后台主页，路由地址 /home
                    this.$router.push('/home')
                }
                if (res.meta.status !== 200) {
                    this.$message.error("登录失败！")
                }
            });
        }
    }
}
</script>

<!-- less :支持less语法格式 scoped 指定样式生效空间（只在当前页面生效） -->
<style lang="less" scoped>
.login_container {
    background-color: #2b4b6b;
    height: 100%;
}

.login_box {
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;
    /*这样box就能在页面上定位到正中间*/
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);

    .avatar-box {
        height: 130px;
        width: 130px;
        border: 1px solid #eee;
        border-radius: 50%;
        padding: 10px;
        /*阴影*/
        box-shadow: 0 0 10px #ddd;
        position: absolute;
        left: 50%;
        transform: translate(-50%, -50%);
        background-color: #fff;

        img {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            background-color: #eee;
        }
    }
}

.login_form {
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 0 20px;
    box-sizing: border-box;
}

.btns {
    display: flex;
    justify-content: flex-end;
}</style>