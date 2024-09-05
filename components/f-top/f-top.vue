<template>
	<!-- 用户简要头部信息 -->
	<view>
		<view class="rowFlex box boxPadding" :class="userSelf ? 'flexRowCenter' : ''">
			<view class="boxLeft avatar">
				<image :src="avatar" mode="aspectFill"></image>
			</view>
			<view class="boxRight topRight" :style="userSelf ? 'height: 140rpx' : ''">
				<view class="nickName">{{nickName}}</view>
				<view class="userInfo">
					<view class="id" v-if="userSelf"><span>用户编号：</span>{{userId}}</view>
					<view class="signMsg"  v-if="!userSelf">
						{{signMsg}}
					</view>
					<view class="qr">
						<image class="userInfoIcon" src="../../static/user/qrCode.svg" mode="aspectFill"></image>
						<image class="userInfoIcon" src="../../static/user/inRight.svg" mode="aspectFill"></image>
					</view>
				</view>
				<view class="cert" v-if="!userSelf">
					<image v-if="health" class="userInfoIcon" src="../../static/tag/health.svg" mode="aspectFill"></image>
					<image v-if="legal" class="userInfoIcon" src="../../static/tag/legal.svg" mode="aspectFill"></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name:"f-top",
		props: {
			// 头像
			avatar: {
				type: String,
				default: "/static/avatar/defaultAvatar.svg"
			},
			// 昵称
			nickName: {
				type: String,
				default: "食域用户"
			},
			// 是否为个人中心展示
			userSelf: {
				type: Boolean,
				default: false
			},
			// 系统内部用户id
			userId: {
				type: String,
				default: null
			},
			// 用户签名
			signMsg: {
				type: String,
				default: "从此你的每一㛑都有我参与"
			},
			// 是否通过健康认证
			health: {
				type: Boolean,
				default: false
			},
			// 是否通过实名认证
			legal: {
				type: Boolean,
				default: false
			}
		},
		data() {
			return {
				
			};
		}
	}
</script>

<style lang="scss" scoped>
	.flexRowCenter {
		align-items: center
	}
	.signMsg {
		// font-size: 28rpx;
		line-height: 35rpx;
		// border: 1px #000 solid;
		color: #777;
		width: 350rpx;
	}
	.cert {
		display: flex;
		align-items: center;
		image {
			width: 45rpx;
			height: 45rpx;
			margin-left: 0;
			margin-right: 30rpx;
		}
	}
	.box {
		background-color: #fff;
		margin-bottom: 20rpx;
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
			border-radius: 20rpx;
		}
	}
	.avatar {
		width: 180rpx;
		height: 180rpx;
		position: relative;
	}
	.boxPadding {
		box-sizing: border-box;
		padding: 30rpx;
	}
	.rowFlex {
		display: flex;
		// align-items: center;
	}
	.nickName {
		font-size: 50rpx;
		font-weight: bold;
	}
	span {
		margin-right: 20rpx;
	}
	.qr {
		display: flex;
		align-items: center;
	}
	.userInfo {
		display: flex;
		align-items: center;
		justify-content: space-between;
		color: #777;
	}
	.userInfoIcon {
		width: 35rpx;
		height: 35rpx;
		margin: 0 15rpx;
	}
	.topRight {
		height: 200rpx;
		width: calc(100% - 200rpx);
	}
	.boxRight {
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		box-sizing: border-box;
		padding-left: 10rpx;
	}
</style>