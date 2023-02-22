<template>
	<view class="">
		<u-navbar title="蜜蜂论文打印" @rightClick="rightClick" :autoBack="false" bgColor="inherit"
			titleStyle="font-weight: 550;">
			<view slot="left" />
		</u-navbar>
		<!-- 头部背景图 -->
		<view class="content">
			<view class="headerView ">
				<image class="logo " src="/static/bg2.png"></image>
				<view class="tabsView u-flex-x-center">
					<u-tabs :list="tabs" @click="tabClick" :activeStyle="activeStyle" :inactiveStyle="inactiveStyle" :itemStyle="itemStyle"  lineWidth="40" lineColor="rgba(255,255,255,.8)"></u-tabs>
				</view>
			</view>
			<view class="contentView">
				
				<view class="listView">
					<view class="empty " v-if="!computedList.length"> 
						<u-empty  
						        mode="order"
						/>
					</view>
					
					<orderItem :list="computedList" v-else  />
					
				</view>
			</view>
		</view>
		
		<!-- 上传文件组件 -->
	</view>
</template>

<script>
	import orderItem from "@/components/orderItem/orderItem.vue"
	export default {
		name: "order",
		components:{
			orderItem,
		},
		data() {
			return {
				activeStyle:{
					color: 'rgba(255,255,255,.8)',
					transform: 'scale(1.05)'
				},
				inactiveStyle:{
					color: 'rgba(255,255,255,.8)',
					transform: 'scale(1)'
				},
				itemStyle:{
					paddingLeft: '15px', paddingRight: '15px', height: '44px',color:'#fff'
				},
				tabs: [{
					name: '全部',
					type:"",
				}, {
					name: '待支付',
					type:0,
				}, {
					name: '打印中',
					type:1
				}, {
					name: '已发货',
					type:2
				}],
				type:"",
				list:[
					{
						status:0,//订单状态  0：待支付，1：打印中，2：已发货
						orderTime:"2023-02-20 19:32:00",//订单日期
						pdfName:"文档名称2023-02-20.pdf",//文件名称
						type:"胶装",//装订类型
						size:"A4",//纸张类型
						cover:"红色",//封面颜色
						sinOrdou:"1",//单双面
						page:11,//文档总页数
						printNums:2,//打印份数
						express:"顺丰快递",//快递公司
						orderNo:"SF20230220193200",//订单号
						addressee:"黄忠胜",//收件人
						phone:18942693171,//手机号
						address:"湖北省武汉市洪山区关南工业园",
						money:"18.00"
					},
					{
						status:1,//订单状态  0：待支付，1：打印中，2：已发货
						orderTime:"2023-02-20 19:32:00",//订单日期
						pdfName:"文档名称2023-02-20.pdf",//文件名称
						type:"胶装",//装订类型
						size:"A4",//纸张类型
						cover:"红色",//封面颜色
						sinOrdou:"1",//单双面
						page:11,//文档总页数
						printNums:2,//打印份数
						express:"顺丰快递",//快递公司
						orderNo:"SF20230220193200",//订单号
						addressee:"黄忠胜",//收件人
						phone:18942693171,//手机号
						address:"湖北省武汉市洪山区关南工业园",
						money:"18.00"
					},
					{
						status:2,//订单状态  0：待支付，1：打印中，2：已发货
						orderTime:"2023-02-20 19:32:00",//订单日期
						pdfName:"文档名称2023-02-20.pdf",//文件名称
						type:"胶装",//装订类型
						size:"A4",//纸张类型
						cover:"红色",//封面颜色
						sinOrdou:"1",//单双面
						page:11,//文档总页数
						printNums:2,//打印份数
						express:"顺丰快递",//快递公司
						orderNo:"SF20230220193200",//订单号
						addressee:"黄忠胜",//收件人
						phone:18942693171,//手机号
						address:"湖北省武汉市洪山区关南工业园",
						money:"18.00"
					}
				]
			}
		},
		computed:{
			computedList(){
				if(this.list.length){
					if(this.type === "")return this.list;
					return this.list.filter((item)=>item.status==this.type)
				}
				return [];
			}
		},
		methods:{
			tabClick({type}){
				this.type = type;
			}
		},
		onLoad() {},
	}
</script>

<style lang="scss">
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		.headerView {
			height: 440rpx;
			position: relative;
			
			.logo {
				width: 100vw;
			}
			.tabsView{
				position: absolute;
				bottom: 0;
				width: 100%;
			}


		}

		.contentView {
			width: 100vw;
			.listView{
				.empty{
						margin-top: 300rpx;
				}
			}
		}
	}
</style>
