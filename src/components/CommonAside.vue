<template>
    <div class="menu-container">
        <el-menu default-active="1-4-1" class="el-menu-vertical-demo" @open="handleOpen" @close="handleClose"
            background-color="#545c64" text-color="#fff" active-text-color="#ffd04b" :collapse="isCollapse">
            <h3>{{ isCollapse ? '后台' : '通用后台管理' }}</h3>
            <el-menu-item @click="clickMenu(item)" v-for="item in noChildren" :key="item.name" :index="item.name">
                <i :class="`el-icon-${item.icon}`"></i>
                <span slot="title">{{ item.label }}</span>
            </el-menu-item>
            <el-submenu v-for="item in hasChildren" :key="item.label" :index="item.label">
                <template slot="title">
                    <i :class="`el-icon-${item.icon}`"></i>
                    <span slot="title">{{ item.label }}</span>
                </template>
                <el-menu-item-group v-for="subItem in item.children" :key="subItem.name">
                    <el-menu-item @click="clickMenu(subItem)" :index="subItem.name">{{ subItem.label }}</el-menu-item>
                </el-menu-item-group>
            </el-submenu>
        </el-menu>
    </div>
</template>

<script>
import Cookie from 'js-cookie';
export default {
    data() {
        return {
            
        };
    },
    methods: {
        handleOpen(key, keyPath) {
            console.log(key, keyPath);
        },
        handleClose(key, keyPath) {
            console.log(key, keyPath);
        },
        // 点击菜单
        clickMenu(item) {
            // console.log(item);
            // 判断当前路径
            if(this.$route.path !== item.path && !(this.$route.path === '/home' && item.path === '/')) {
                this.$router.push(item.path);
            }
            this.$store.commit('selectMenu', item);
        }
    },
    computed: {
        noChildren() {
            return this.menuData.filter((item) => !item.hasOwnProperty("children"));
        },
        hasChildren() {
            return this.menuData.filter((item) => item.hasOwnProperty("children"));
        },
        isCollapse() {
            return this.$store.state.tab.isCollapse;
        },
        menuData() {
            // 判断当前数据，如果缓存没有，去store中获取
            return JSON.parse(Cookie.get('menu')) || this.$store.state.tab.menu;
        }
    },
};

</script>
<style lang="scss" scoped>
.menu-container {
    height: 100%;
    .el-menu-vertical-demo:not(.el-menu--collapse) {
        width: 200px;
        min-height: 400px;
    }
    .el-menu {
        height: 100vh;
        border-right: none;
        h3 {
            color: #fff;
            text-align: center;
            line-height: 48px;
            font-size: 16px;
            font-weight: 400;
        }
    }
}
</style>