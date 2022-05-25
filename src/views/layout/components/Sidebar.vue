<template>
    <div>
        <el-card shadow="never">
            <el-menu :default-active="active" @select="onSelect">
                <el-menu-item v-for="item in constantRouterMap" v-if="item.meta&&item.meta.type=='user'&&(token||!item.meta.LoginRequired)&&(!mini||!item.meta.mini)"
                    :key="item.path" :index="item.path">
                    <i :class="item.meta.icon"></i>
                    <span slot="title">{{item.meta.title}}</span>
                </el-menu-item>
            </el-menu>
        </el-card>

        <el-card shadow="never" style="margin-top: 20px;text-align: center">
            <div v-if="!token" style="font-size: 0.9rem;line-height: 1.5;color: #606c71;">
                &nbsp;&nbsp; 未登录&nbsp;&nbsp;
                <el-button type="text" @click="openTokenDialog">登录</el-button>
            </div>
            <div v-if="token" style="font-size: 0.9rem;line-height: 1.5;color: #303133;">
                &nbsp;&nbsp; 已登录&nbsp;&nbsp;
                <el-button type="text" @click="cancellation">注销</el-button>
            </div>
            <div style="margin-top: 10px;text-align: left">
                <!-- <el-alert title="Token获取" type="info" description="在 github-> settings-> developerSettings-> personalAccessTokens 勾选gist权限,获取Token. 详情参考README.md"
                    :closable="false">
                </el-alert> -->
                <img alt="今日诗词" src="https://v2.jinrishici.com/one.svg" style="max-width:100%; display: block; margin: 0 auto;">
            </div>
        </el-card>
        <token-dialog ref="tokenDialog"></token-dialog>
    </div>
</template>

<script>
    import { mapGetters } from 'vuex'
    import { constantRouterMap } from '@/router'
    import TokenDialog from '@/views/common/TokenDialog'
    export default {
        components: {
            TokenDialog
        },
        data() {
            return {
                constantRouterMap,
                active: "",
                parentUrl: "",
                menuList: []
            }
        },
        computed: {
            ...mapGetters([
                'token',
                'githubUsername',
                'mini'
            ])
        },
        mounted() {
            let arr = this.$route.path.split("/")
            this.active = "/" + arr[1] + "/" + arr[2]
        },
        methods: {
            onSelect(index) {
                this.$router.push(index)
            },
            openTokenDialog() {
                this.$refs.tokenDialog.open(() => {

                })
            },
            cancellation() {
                this.$store.dispatch("Cancellation")
            }
        }
    }
</script>
