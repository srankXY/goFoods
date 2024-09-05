<template>
	<!-- 厨师端商品详情里面展示的菜品详情 -->
	<view class="out">
		<view class="food">
			<view class="foodLeft" :class="!foodMaterjal ? 'cartHight' : 'foodHight'">
				<image :src="foodImg" mode="aspectFill"></image>
			</view>
			<view class="foodRight" :class="!foodMaterjal ? 'cartHight' : 'foodHight'">
				<view class="foodName">
					{{foodName}}
				</view>
				<view class="foodMaterjal" v-if="foodMaterjal != null">
					<!-- 配料 -->
					配料： {{foodMaterjal}}
				</view>
				<view class="foodPrice">
					<span>￥{{foodPrice}}</span>
					<u-number-box integer min=0 :value="foodCount" class="numberBox" @change="addCart"></u-number-box>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name:"f-food",
		props: {
			foodId: {
				type: Number
			},
			foodImg: {
				type: String
			},
			foodMaterjal: {
				type: String
			},
			foodClass: {
				type: String
			},
			foodName: {
				type: String
			},
			foodPrice: {
				type: Number
			},
			foodCount: {
				type:Number,
				default: 0
			}
		},
		data() {
			return {
			};
		},
		methods: {
			addCart(e) {
				this.$emit('addCart', {
					foodId: this.foodId,
					foodImg: this.foodImg,
					foodName: this.foodName,
					foodPrice: this.foodPrice,
					foodCount: e.value
				})
			}
		},
	}
</script>

<style lang="scss" scoped>
	.foodMaterjal {
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 2;
		-webkit-box-orient: vertical;
		display: -moz-box;
		-moz-line-clamp: 2;
		-moz-box-orient: vertical;
		overflow-wrap: break-word;
		word-break: break-all;
		white-space: normal;
		overflow: hidden;
		font-size: 28rpx;
		color: #555;
	}
	.foodHight {
		height: 200rpx;
		width: 200rpx;
	}
	.cartHight {
		height: 120rpx;
		width: 120rpx;
	}
	.foodLeft {
		image {
			width: 100%;
			height: 100%;
			border-radius: 10rpx;
		}
	}
	.out {
		padding-top: 10rpx;
		padding-bottom: 10rpx;
		margin: 0 10rpx;
		margin-bottom: 20rpx;
		border-radius: 20rpx;
	}
	.food {
		// border: 1px solid #000;
		display: flex;
		align-items: center;
		justify-content: space-between;
		background-color: #fff;
		
		.foodRight {
			flex: 1;
			margin-left: 34rpx;
			display: flex;
			flex-direction: column;
			justify-content: space-between;
			.foodName {
				// border: 1px solid #000;
				font-size: 36rpx;
				font-weight: bold;
			}
			.foodPrice {
				display: flex;
				align-items: center;
				justify-content: space-between;
				span {
					font-size: 36rpx;
					font-weight: bold;
					color: #fb8106;
					// margin-right: 60rpx;
				}
			}
		}
	}
</style>