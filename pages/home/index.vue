<template>
	<view class="content">
		<u-navbar title="蜜蜂论文打印" @rightClick="rightClick" :autoBack="false" bgColor="inherit"
			titleStyle="font-weight: 550;">
			<view slot="left" />
		</u-navbar>
		<!-- 头部背景图 -->
		<view class="headerView ">
			<image class="logo " src="/static/bg.png"></image>
			<view class="contentTxt">
				<u--text text="蜜蜂论文打印，您的私人论文打印专家~" align="center" size="18" color="rgba(255,255,255,.8)"></u--text>
			</view>
		</view>
		<view class="updataView u-flex u-flex-middle hzs_flex_column">
			<u--image :showLoading="true" :src="src" width="370rpx" height="370rpx" @click="upFile"></u--image>
		</view>
		<view class="">
			<u-radio-group v-model="radiovalue1" placement="column" @change="goAgreement">
				<u-radio activeColor="#fe6d15" :customStyle="{marginBottom: '8px'}" label="我已阅读并已同意以下协议" name="协议" />
			</u-radio-group>
		</view>

		<!-- 上传文件组件 -->
	</view>
</template>

<script>
	import {
		getUserInfo,
		userLogin
	} from "@/api/user.js"
	export default {
		name: "index",
		data() {
			return {
				src: "/static/upFile.png",
				radiovalue1: "协议"
			}
		},
		onLoad() {
			// this.getdata()
			console.log(this.$util.isLogin());
		},
		methods: {
			goAgreement() {
				uni.$u.route('/pages/agreement/agreement')
			},
			upFile() {
				const that = this;
				this.loading = true;
				// #ifdef MP
				wx.chooseMessageFile({
					type: 'file',
					count: 1,
					extension: ['pdf'], //可自定义过滤文件
					success({
						tempFiles
					}) {
						// 处理loading过快消失的bug，可能和微信执行的顺序有关，打开上传文件会刷新页面
						setTimeout(() => {
							uni.showLoading({
								title: '文件上传中...',
								mask: true
							})
							uni.$u.route({
								type: "to",
								url: "/pages/configFile/configFile",
								params: tempFiles[0]
							})
						}, 500)

					},
					fail(err) {
						uni.hideLoading();
					}
				})
				// #endif

			}
		}
	}
</script>

<style lang="scss">
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;

		.headerView {
			min-height: calc(50vh - 124rpx);
			width: 100vw;
			position: relative;
			.logo {
				width: 100vw;
			}
			.contentTxt{
				position: absolute;
				top: calc(50% - 60rpx);
				width: calc(100vw - 60rpx);
				padding:30rpx;
				text-align: center;
			}
		}
	}

	.updataView {
		min-height: calc(50vh);
		width: 100vw;

	}
</style>
