<template>
    <div>
        <el-button type="info" @click="logout">退出</el-button>
    </div>
</template>

<script>
export default {
    methods: {
        logout() {
            // 清空session
            window.sessionStorage.clear();
            // 重定向到login
            this.$router.push("/login");
        }
    }
};
</script>

<style lang="less" scoped></style>