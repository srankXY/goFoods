<template>
	<view>
		<view class="goinBox" :style="contentImg ? 'height: 150rpx' : ''">
			<view class="boxLeft">
				<!-- <view class="funcIconView"> -->
				<image v-if="funcIcon" :src="funcIcon" mode="aspectFill"></image>
				<!-- </view> -->
				<view class="funcName" :style="!funcIcon ? 'margin: 0' : ''">
					{{funcName}}
				</view>
			</view>
			<view class="boxRight">
				<span v-if="content" class="content">{{content}}</span>
				<image v-if="contentImg" class="contentImg"
				 :style="iconContent ? 'width: 40rpx; height: 40rpx; border-radius: 0' : ''"
				 :src="contentImg" mode="aspectFill"></image>
				<image class="funcArrowRight" src="../../static/user/inRight.svg" mode="aspectFill"></image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name:"f-goin",
		props: {
			funcName: {
				type: String,
				default: '食域'
			},
			// 路径必须是绝对路径或者使用require()调用相对路径
			funcIcon: {
				type: String,
				default: ''
			},
			contentImg: {
				type: String,
				default: ''
			},
			content: {
				type: String,
				default: ''
			},
			iconContent: {
				type: Boolean,
				default: false
			}
			
		},
		data() {
			return {
				
			};
		},
	}
</script>

<style lang="scss" scoped>
	.goinBox:active {
		background-color: #ddd;
	}
	.goinBox {
		display: flex;
		align-items: center;
		justify-content: space-between;
		background-color: #fff;
		// margin: 20rpx 0;
		box-sizing: border-box;
		padding-left: 50rpx;
		padding-right: 45rpx;
		height: 120rpx;
		// border: 1px #000 solid;
		.boxLeft {
			display: flex;
			align-items: center;
			image {
				width: 55rpx;
				height: 55rpx;
			}
		}
		.boxRight {
			display: flex;
			align-items: center;
			.contentImg {
				width: 130rpx;
				height: 130rpx;
				border-radius: 20rpx;
				margin: 0 10rpx;
			}
			.content {
				color: #777;
				margin: 0 10rpx;
			}
		}
		.funcArrowRight {
			width: 35rpx;
			height: 35rpx;
			padding-right: 5rpx;
		}
	}
	.funcName {
		margin: 0 30rpx;
		font-size: 34rpx;
	}
</style>