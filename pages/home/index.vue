<template>
	<view class="content">
		<u-navbar title="蜜蜂论文打印" @rightClick="rightClick" :autoBack="false" bgColor="inherit"
			titleStyle="font-weight: bold;">
			<view slot="left" />
		</u-navbar>
		<!-- 头部背景图 -->
		<view class="headerView ">
			<image class="logo " src="/static/bg.png"></image>
		</view>
		<view class="updataView u-flex u-flex-middle">
			<u--image :showLoading="true" :src="src" width="370rpx" height="370rpx" @click="upFile"></u--image>

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
				src: "/static/upFile.png"
			}
		},
		onLoad() {
			// this.getdata()
			console.log(this.$util.isLogin());
		},
		methods: {
			upFile() {
				const that = this;
				this.loading = true;
				// #ifdef MP
				wx.chooseMessageFile({
					type: 'file',
					count: 1,
					extension: ['pdf'], //可自定义过滤文件
					success(res) {
						console.log('msgfile', res);
						// 处理loading过快消失的bug，可能和微信执行的顺序有关，打开上传文件会刷新页面
						setTimeout(() => {
							uni.showLoading({
								title: '文件上传中...',
								mask: true
							})
						}, 500)
						wx.uploadFile({
							url: that.$u.api.getUploadCVFileUrl, //上传api地址
							filePath: res.tempFiles[0].path,
							name: "file",
							header: {
								"content-type": "application/x-www-form-urlencoded",
								xToken: that.$store.state.vuex_token
							},
							formData: {
								type: 'common'
							},
							success(data) {
								// 微信返回的数据被转成string
								const curData = JSON.parse(data.data);
								if (curData.code === 200) {
									// that.form.lastCV.cvUrl = JSON.parse(data.data).result.path;
									// that.form.lastCV.cvName = res.tempFiles[0].name;
									// that.form.lastCV.cvSize = (res.tempFiles[0].size / 1024).toFixed(2);
								} else {
									that.$u.toast(decodeURIComponent(curData.msg), 3000);
								}
								uni.hideLoading();
								// uni.hideLoading();
							},
							fail(err) {
								uni.hideLoading();
							}

						})
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
			min-height: 50vh;
			width: 100vw;
			.logo {
				width: 100vw;
			}
		}
	}

	.updataView {
		min-height: 50vh;
		width: 100vw;
		
	}

	
</style>
