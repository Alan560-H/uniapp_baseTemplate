<template>
	<view class="order">
		<view class="orderItem hzs_marginTop_20" v-for="item in list" :key="item.orderNo">
			<view class="orderTop ">
				<view class="leftContent u-flex">
					<view class="IconView ">
						<view >
							<u-icon name="clock" size="16"></u-icon>
						</view>
						<view class="iconHeight">
							<u-icon name="●" size="16" color="#fe6d15"></u-icon>
						</view>
					</view>
					<view class="contentMain">
						<view class="orderTime">
							<u--text :text="item.orderTime"></u--text>
						</view>
						<view class="fileName">
							<u--text :text="item.pdfName"></u--text>
						</view>

						<view class="u-flex infoson">
							<u--text type="info" size="12" :text="item.sinOrdou"></u--text>
							<u--text type="info" size="12" :text="item.cover"></u--text>
						</view>
					</view>
				</view>
				<view class="orderStatus ">
					<u-tag :text="computedTag(item.status)"  type="success" shape="circle" />
					<u--text  mode="price" size="18" color="#fe6d15" :text="item.money"></u--text>
				</view>
			</view>
			<view class="orderBottom hzs_marginTop_10" v-if="item.status==2">
				<view class="hzs_marginTop_10">
					<view class="orderBottomConet ">
						<view class="leftContent u-flex">
							<view class="">
							<u-icon name="●" size="16"></u-icon>
							</view>
						</view>
						<!-- 快递信息 -->
						<view class="rightContent"- >
							<view class="orderInfo u-flex">
								<u--text type="info" prefixIcon="快递公司：" iconStyle="font-size:24rpx;margin-right:10rpx;"
									size="12" :text="item.express"></u--text>
								<view class="hhh u-flex">
									<u--text type="info" size="12" :text="item.orderNo" @click="setClipboardData(item.orderNo)"></u--text>
								</view>
							</view>
							<view class="orderInfo u-flex hzs_marginTop_10">
								<u--text type="info" prefixIcon="收件信息：" iconStyle="font-size:24rpx;margin-right:10rpx;"
									size="12" :text="item.address"></u--text>
							</view>
							
							
							<view class="orderInfo u-flex hzs_marginTop_10">
								<u--text type="info" prefixIcon="收  件 人：" iconStyle="font-size:24rpx;margin-right:10rpx;"
									size="12" :text="item.addressee"></u--text>
								<view class="hhh u-flex">
									<u--text type="info" size="12" :text="item.phone" @click="setClipboardData(item.orderNo)"></u--text>
								</view>
							</view>
						</view>
						
						
						
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "orderItem",
		props: {
			list: {
				type: Array,
				default: () => []
			}
		},
		computed: {
			computedTag() {
				return (Tag) => {
					switch (Tag) {
						case 0:
							return "待支付"
						case 1:
							return "打印中"
						case 2:
							return "已发货"
						default:
							return "待支付"
					}
				}
			}
		},
		data() {
			return {
				iconStyle: {
					lineHeight: '32rpx',
					marginRight: '10rpx',
					fontSize: '52rpx',
					color: '#fe6d15'
				},
				iconStyle2: {
					lineHeight: '32rpx',
					marginRight: '10rpx',
					fontSize: '52rpx',
					color: '#979694'
				}
			};
		},
		onLoad() {
			console.log("hello world");
		},
		methods:{
			setClipboardData(text){
				wx.setClipboardData({
					data:text,
					success(){
						uni.showToast({
							title:"复制成功",
						})
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	.order {
		padding: 30rpx;

		.orderItem {
			padding: 30rpx;
			border: 1px #f5f2f3 solid;
			box-shadow: 0px 0px 5px 1px rgba(0, 0, 0, .1);
			border-radius: 20rpx;

			.orderTop {
				display: flex;
				justify-content: space-between;
				.orderStatus{
					display: flex;
					flex-direction: column;
					
				}
				.leftContent {
					.iconHeight{
						min-height: 36rpx;
						margin-top: 10rpx;
					}
					.contentMain {
						margin-left: 20rpx;

						.orderTime {
							line-height: 32rpx;
						}

						.fileName {
							line-height: 52rpx;
						}
					}
				}

			}

			.orderBottom {
				min-height: 120rpx;
				border-top: 2px dashed #e7e3e2;

				.orderBottomConet {
					.orderInfo {
						margin-left: 20rpx;
						.hhh{
							margin-left: 20rpx;
						}
					}

					display: flex;
					.rightContent{
						width: 100%;
					}
				}
			}

			.info {
				margin-left: 40rpx;
				display: flex;
				justify-content: space-between;

				.infoson {
					min-width: 300rpx;
				}
			}
		}
	}
</style>
