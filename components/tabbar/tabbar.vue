<template>
    <view>
        <u-tabbar :value="current?current:0" @change="changeTab" :fixed="true" :placeholder="false"
                  :safeAreaInsetBottom="true" :border="false">
            <u-tabbar-item v-for="(item,index) in list" :key="index" :text="item.text">
                <image class="u-page__item__slot-icon" slot="active-icon" :src="item.iconPath"></image>
                <image class="u-page__item__slot-icon" slot="inactive-icon" :src="item.selectedIconPath"></image>
            </u-tabbar-item>
        </u-tabbar>
    </view>
</template>
 
<script>
    export default {
        name: "tabbar",
        props: {
            current: Number
        },
        created() {
            //隐藏原生tabbar
            uni.hideTabBar()
        },
        data() {
            return {
                list: [{
                    selectedIconPath: "../../static/logo.png",
                    iconPath: "../../static/logo.png",
                    text: '首页',
                    customIcon: false,
                    pagePath: "pages/home/index"
                },
                    {
                        selectedIconPath: "../../static/logo.png",
                        iconPath: "../../static/logo.png",
                        text: '订单',
                        customIcon: false,
                        pagePath: "pages/order/index"
                    },
                    {
                        selectedIconPath: "../../static/logo.png",
                        iconPath: "../../static/logo.png",
                        text: '我的',
                        customIcon: false,
                        pagePath: "pages/my/index"
                    }
                ],
            }
        },
        methods: {
            changeTab(e) {
                console.log(e)
                uni.switchTab({
                    url: '/' + this.list[e].pagePath,
                })
            }
        }
    }
</script>
 
<style scoped>
    .u-page__item__slot-icon{
        width: 20px!important;
        height: 20px!important;
    }
</style>