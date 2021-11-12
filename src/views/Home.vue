<template>
    <el-container>
        <el-aside width="200px">
            <SideMenu></SideMenu>
        </el-aside>
        <el-container>
            <el-header>
                <strong>VueAdmin后台管理系统</strong>

                <div class="header-avatar">
                    <el-avatar size="medium" :src="userInfo.avatar"></el-avatar>

                    <el-dropdown>
            <span class="el-dropdown-link">
              {{ userInfo.username }}<i class="el-icon-arrow-down el-icon--right"></i>
            </span>
                        <el-dropdown-menu slot="dropdown">
                            <el-dropdown-item>
                                <router-link to="/userCenter">个人中心</router-link>
                            </el-dropdown-item>
                            <el-dropdown-item @click.native="logout">退出</el-dropdown-item>
                        </el-dropdown-menu>
                    </el-dropdown>

                    <el-link href="https://markerhub.com" target="_blank">网站</el-link>
                    <el-link href="https://space.bilibili.com/64519748" target="_blank">B站</el-link>
                </div>

            </el-header>
            <el-main>
                <Tabs></Tabs>
                <div style="margin: 0 15px">
                    <router-view></router-view>
                </div>
            </el-main>
        </el-container>
    </el-container>
</template>

<script>
import SideMenu from "./inc/SideMenu";
import Tabs from "./inc/Tabs";

export default {
    name: "Home",
    components: {Tabs, SideMenu},
    comments: {
        SideMenu,
        Tabs
    },
    data() {
        return {
            userInfo: {
                id: "",
                username: "",
                avatar: ""
            }
        }
    },
    created() {
        this.getUserInfo()
    },
    methods: {
        getUserInfo() {
            this.$axios.get("/sys/userInfo").then(res => {
                this.userInfo = res.data.data
            })
        },
        logout() {
            this.$axios.post("/logout").then(res => {
                localStorage.clear()
                sessionStorage.clear()

                this.$store.commit("resetState")

                this.$router.push("/login")
            })
        }
    }
}
</script>

<style scoped>
.el-container {
    margin: 0;
    padding: 0;
    height: 100%;
}

.header-avatar {
    float: right;
    width: 210px;
    display: flex;
    justify-content: space-around;
    align-items: center;
}

.el-dropdown-link {
    cursor: pointer;
}

.el-header {
    background-color: #17B3A3;
    color: #333;
    text-align: center;
    line-height: 60px;
}

.el-aside {
    background-color: #D3DCE6;
    color: #333;
    line-height: 200px;
}

.el-main {
    color: #333;
    padding: 0;
}

a {
    text-decoration: none;
}
</style>