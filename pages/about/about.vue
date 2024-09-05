<template>
	<!-- 仿微信我页面 -->
	<view>
		<!-- 顶部个人信息 -->
		<view class="box rowFlex boxPadding" @click="jumpToPage(userInfoPage.urlPath)">
			<view class="boxLeft avatar">
				<image src="../../static/avatar/defaultAvatar.svg" mode="aspectFill"></image>
				<view class="activeStyle"></view>
			</view>
			<view class="boxRight topRight">
				<view class="nickName">{{userInfo.nickName}}</view>
				<view class="userInfo">
					<view class="id"><span>用户编号：</span>{{userInfo.foodIdCard}}</view>
					<view class="qr">
						<image class="userInfoIcon" src="../../static/user/qrCode.svg" mode="aspectFill"></image>
						<image class="userInfoIcon" src="../../static/user/inRight.svg" mode="aspectFill"></image>
					</view>
				</view>
			</view>
		</view>
		<!-- 进行中的订单 -->
		<view class="box boxPadding">
			<!-- 多个订单使用轮播图swiper -->
			<view class="orderProgress"></view>
		</view>
		<!-- 设置、地址管理等功能 -->
		<view class="box">
			<view class="rowFlex flexBetween funcPadding" v-for="item of funcList.slice(0,2)" :key="item.id" @click="jumpToPage(item.urlPath)">
				<view class="boxLeft">
					<view class="funcIconView">
						<image :src="item.icon" mode="aspectFill"></image>
					</view>
					<view class="funcName">
						{{item.name}}
					</view>
				</view>
				<view class="boxRight">
					<image class="funcArrowRight userInfoIcon" src="../../static/user/inRight.svg" mode="aspectFill"></image>
				</view>
			</view>
		</view>
		<view class="box boxBottom">
			<view class="rowFlex flexBetween funcPadding" v-for="item of funcList.slice(2,funcList.length-1)" :key="item.id" @click="jumpToPage(item.urlPath)">
				<view class="boxLeft">
					<view class="funcIconView">
						<image :src="item.icon" mode="aspectFill"></image>
					</view>
					<view class="funcName">
						{{item.name}}
					</view>
				</view>
				<view class="boxRight">
					<image class="funcArrowRight userInfoIcon" src="../../static/user/inRight.svg" mode="aspectFill"></image>
				</view>
			</view>
		</view>
		<view class="box boxBottom">
			<view class="rowFlex flexBetween funcPadding" v-for="item of funcList.slice(funcList.length-1,funcList.length)" :key="item.id" @click="jumpToPage(item.urlPath)">
				<view class="boxLeft">
					<view class="funcIconView">
						<image :src="item.icon" mode="aspectFill"></image>
					</view>
					<view class="funcName">
						{{item.name}}
					</view>
				</view>
				<view class="boxRight">
					<image class="funcArrowRight userInfoIcon" src="../../static/user/inRight.svg" mode="aspectFill"></image>
				</view>
			</view>
		</view>
	</view>
	
</template>

<script>
	import fGlobal from '../../global.vue'
	export default {
		data() {
			return {
				userInfo: {
					id: 1,
					foodIdCard: "000001",
					nickName: "肖扬",
					avatar: null,
				},
				userInfoPage: {
					name: "个人信息",
					urlPath: "/pages/about/userInfo/userInfo"
				},
				funcList: [
					{
						id: 0,
						name: "钱包",
						icon: "../../static/funcIcon/wallet.svg",
						urlPath: "/pages/about/wallet/wallet"
					},
					{
						id: 1,
						name: "会员中心",
						icon: "../../static/funcIcon/vip.svg",
						urlPath: "/pages/about/vip/vip"
					},
					{
						id: 2,
						name: "订单管理",
						icon: "../../static/funcIcon/order.svg",
						urlPath: ""
					},
					{
						id: 7,
						name: "我的发布",
						icon: "../../static/funcIcon/push.svg",
						urlPath: ""
					},
					// 地址管理和电话管理应放置到头像区
					// {
					// 	name: "地址管理",
					// 	icon: "",
					// 	urlPath: ""
					// },
					{
						id: 3,
						name: "认证中心",
						icon: "../../static/funcIcon/safe.svg",
						urlPath: "/pages/about/cert/cert"
					},
					{
						id: 4,
						name: "免责申明",
						icon: "../../static/funcIcon/hint.svg",
						urlPath: ""
					},
					{
						id: 5,
						name: "关于食域",
						icon: "../../static/funcIcon/about.svg",
						urlPath: ""
					},
					{
						id: 6,
						name: "设置",
						icon: "../../static/funcIcon/set.svg",
						urlPath: "/pages/about/set/set"
					}
				]
			};
		},
		onLoad() {
			uni.uni.request({
				url: fGlobal.API.baseUrl+fGlobal.API.userInfoPath,
				method: 'GET',
				data: {},
				success: res => {
					this.userInfo = res.data.data
				},
				fail: () => {},
				complete: () => {}
			});
		},
		computed: {
		},
		methods: {
			jumpToPage(url) {
				console.log(url)
				uni.navigateTo({
					url: url,
					success: (res) => {
						res.eventChannel.emit('user-id', { userID: this.userInfo.id })
					}
				})
			},
			showPopus() {
				
			},
		},
	}
</script>

<style lang="scss" scoped>
	.activeStyle {
		width: 100%;
		height: 100%;
		opacity: 0;
		background-color: #000;
		position: absolute;
		z-index: 9999;
		left: 0;
		top: 0;
		border-radius: 20rpx;
	}
	.activeStyle:active {
		opacity: 0.2;
	}
	.funcName {
		margin: 0 30rpx;
		font-size: 34rpx;
	}
	.topRight {
		height: 140rpx;
		width: calc(100% - 200rpx);
	}
	.funcIconView {
		width: 55rpx;
		height: 55rpx;
	}
	.rowFlex {
		display: flex;
		align-items: center;
	}
	.flexBetween {
		justify-content: space-between;
	}
	.boxPadding {
		box-sizing: border-box;
		padding: 30rpx;
	}
	.funcPadding {
		// margin: 40rpx 0;
		box-sizing: border-box;
		padding: 0 30rpx;
		height: 120rpx;
		// background-color: #eee;
		// border: 1px solid #000;
	}
	.funcPadding:active {
		background-color: #ddd;
	}
	.box {
		background-color: #fff;
		margin: 20rpx 0;
		// border: 1px solid #000;
		
		.avatar {
			width: 180rpx;
			height: 180rpx;
			position: relative;
		}
		
		.boxLeft {
			display: flex;
			align-items: center;
			// border: 1px solid #000;
			border-radius: 20rpx;
			margin: 0 20rpx;
			image {
				width: 100%;
				height: 100%;
			}
		}
		.boxRight {
			display: flex;
			flex-direction: column;
			justify-content: space-between;
			box-sizing: border-box;
			padding-left: 10rpx;
			// border: 1px solid #000;
			.funcArrowRight {
				// box-sizing: border-box;
				padding-right: 5rpx;
			}
			.nickName {
				font-size: 50rpx;
				font-weight: bold;
			}
			.userInfoIcon {
				width: 35rpx;
				height: 35rpx;
				margin: 0 15rpx;
			}
			.userInfo {
				display: flex;
				align-items: center;
				justify-content: space-between;
				color: #777;
				span {
					margin-right: 20rpx;
				}
				.qr {
					display: flex;
					align-items: center;
				}
			}
		}
	}
</style>
