<template>
	<view class="loginContainer ">
		<!-- 头像部分 -->
		<view class="baseStyle u-flex u-flex-middle hzs_flex_column ">
			<u--image :showLoading="true" src="/static/logo.png" width="80px" height="80px"></u--image>
			<view class="AvatarText hzs_marginTop_20">
				<u--text text="蜜蜂论文打印" bold size="16"></u--text>
			</view>
		</view>
		<!-- 授权按钮组 -->
		<view class="baseStyle u-flex u-flex-y-center hzs_flex_column ">
			<view class="">
				<u--text type="info" size="14" text="授权登录后开始打印"></u--text>
			</view>
			<view class="buttonView hzs_marginTop_20 ">
					<u-button type="success" text="微信授权登录" icon="weixin-fill" @click="open">
					</u-button>
			</view>
		</view>
		<u-popup :show="show" @close="close" @open="open" mode="bottom" :customStyle="nickName" round="10">
			<view class="">
				<view class="u-flex">
					<u-avatar src="/static/logo.png" size="30"></u-avatar>
					<u--text text=" 蜜蜂论文打印 申请获得以下权限" size="14" type="info"></u--text>
				</view>
				<view class="hzs_marginTop_10">
					<u--text text="请点击选择您的头像,输入您的昵称。否则将使用默认头像和默认昵称" size="16"></u--text>
					<view class="hzs_marginTop_10">
						<u--text text="主要用于小程序登录、注册功能" size="14" type="info"></u--text>

					</view>
				</view>
			</view>
			<view class="u-flex u-flex-middle hzs_flex_column ">
				<view class="u-flex u-flex-middle hzs_flex_column">
					<view class="avatarView">
						<view>
							<u-avatar :src="avatarUrl" size="60"></u-avatar>
						</view>
						<view class="btnAbso">
							<button type="default" open-type="chooseAvatar" @chooseavatar="chooseavatar">
								h
							</button>
						</view>
					</view>
					<!-- 这里快捷选择微信用户名得话 ，模拟器是读取不到快捷输入得。手机就可以 -->
					<u--input type="nickname" placeholder="默认昵称微信用户" border="surround" v-model="nickNameValue"
						@change="change"></u--input>
				</view>
				<view class="hzs_marginTop_10" style="width:100%">
					<u-button type="success" text="确定授权" @click="upLoadUserInfo"></u-button>
				</view>
			</view>

		</u-popup>
		<u-toast ref="uToast"></u-toast>
	</view>
</template>

<script>
	import {
		upDataUserInfo
	} from "@/api/user.js"
	export default {
		data() {
			return {
				avatarUrl:"",
				show: false,
				nickName: {
					padding: "30rpx",
				},
				nickNameValue: "",
			};
		},
		methods: {
			change(nickName) {
				this.nickNameValue = nickName
			},
			// 上传用户信息
			upLoadUserInfo() {
				let _this = this;
				this.show = false;
				let userInfo = {
					avatar: this.avatarUrl,
					nickName: this.nickNameValue == "" ? "微信用户" : this.nickNameValue,
				}
				uni.login({
					"provider": "weixin",
					"onlyAuthorize": true, // 微信登录仅请求授权认证
					success: function(event) {
						const {
							code
						} = event
						console.log(userInfo.nickName, "??");
						_this.$refs.uToast.show({
							message: userInfo.nickName,
							position: "center",
							type: 'error',
							icon: false,
							duration: 2000,
						})
						uni.setStorageSync('token', "tempToken")
						//客户端成功获取授权临时票据（code）,向业务服务器发起登录请求。
						// uni.request({
						// 	url: 'https://www.example.com/loginByWeixin', //仅为示例，并非真实接口地址。
						// 	data: {
						// 		code: event.code
						// 	},
						// 	success: (res) => {
						// 		//获得token完成登录
						// 		uni.setStorageSync('token', res.token)
						// upDataUserInfo({
						// ...userInfo,
						// openid:res.openid
						// })
						// 		// 接下来是上传用户数据
						// 	}
						// });
					},
					fail: function(err) {
						// 登录授权失败  
						// err.code是错误码
					}
				})


			},
			// 关闭模态框
			close() {
				this.show = false;
			},
			open() {
				this.show = true;
			},
			chooseavatar(e) {
				// this.uploadFilePromise(e.detail.avatarUrl)
				console.log(e, uni.$u);
				this.avatarUrl = e.detail.avatarUrl;
				// uni.$u.route({
				// 	type: "reLaunch",
				// 	url: "/pages/home/index"
				// })
			},
			uploadFilePromise(value) {
				uni.uploadFile({
					url: '开发者服务器',
					filePath: "临时路径",
					name: 'file',
					success(res) {
						const data = res.data
						//do something
						completeMemberInfo(res.data.avatarUrl) //参数返回依接口而定

					}
				})
			}
		},
	};
</script>

<style lang="scss">
	// 基础样式设置
	.baseStyle {
		height: 50vh;
		width: 100%;

		.buttonView {
			width: calc(100% - 400rpx);
		}
	}
	.avatarView{
		    position: relative;
		    width: 60px;
			padding: 20rpx;
			.btnAbso{
				    position: absolute;
				    top: 10px;
					opacity: 0;
					width: 60px;
			}
	}
</style>
