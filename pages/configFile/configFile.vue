<template>
	<view>
		{{filePath||"hello"}}
	</view>
</template>

<script>
	export default {
		data() {
			return {
				filePath:"",
			}
		},
		onLoad(file){
			console.log(file,"hh");
			let that = this;
			return;
			wx.openDocument({
			    filePath: file.path,   // 装载对应文件的路径
			    fileType: 'pdf',   // 指定打开的文件类型 我写的固定类型 也可根据文件的后缀动态设置
			    showMenu: true,       // 右上角的菜单转发分享操作
			    success: function (res) {
			        that.$u.toast(filePath, 3000);
					this.filePath = file.path
			    },
			    fail: function (err) {
			        that.$u.toast(filePath, 3000);
			    }
			})
			return;
			
				let downloadTask = wx.downloadFile({//下载对应文件
			        url: file.path, // 下载文件网络地址
			        success: function (res) {
			            var filePath = res.tempFilePath;//本地文件路径    
			            wx.openDocument({
			                filePath: filePath,   // 装载对应文件的路径
			                fileType: 'pdf',   // 指定打开的文件类型 我写的固定类型 也可根据文件的后缀动态设置
			                showMenu: true,       // 右上角的菜单转发分享操作
			                success: function (res) {
			                    that.$u.toast(filePath, 3000);
			                },
			                fail: function (err) {
			                    that.$u.toast(filePath, 3000);
			                }
			            })
			        }
			    })
			  downloadTask.onProgressUpdate((res) => {
			       console.log('下载进度', res.progress)
			   })

		},
		methods: {
			upFile(){
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
			}
		}
	}
</script>

<style>

</style>
