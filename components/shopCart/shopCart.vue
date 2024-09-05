<template>
	<view class="cartBox" :class="!popupState ? 'close' : ''">
		<view class="title">购物车</view>
		<view class="content">
			<scroll-view scroll-y="true" class="scrollHeight">
				<view>
					<f-food
					v-for="foodObj in foodsArr"
					:food-id="foodObj.foodId"
					:food-name="foodObj.foodName"
					:food-img="foodObj.foodImg"
					:food-price="foodObj.foodPrice"
					:food-count="foodObj.foodCount"
					:key="foodObj.foodId"
					@addCart="addCart">
					</f-food>
				</view>
			</scroll-view>
		</view>
	</view>
</template>

<script>
	export default {
		name:"shopCart",
		props: {
			foodsArr: {
				type: Array
			},
			popupState: {
				type: Boolean
			}
		},
		data() {
			return {
				
			};
		},
		methods: {
			addCart(e) {
				this.$emit('addCart', e)
			}
		}
	}
</script>

<style lang="scss" scoped>
	.title {
		background-color: #32cd3210;
		height: 60rpx;
		line-height: 60rpx;
		text-align: center;
		font-size: 26rpx;
		border-radius: 30rpx 30rpx 0 0;
	}
	.cartBox {
		position: fixed;
		bottom: 0rpx;
		width: 100%;
		height: 40%;
		transition: all 0.5s;
		background-color: #fff;
		border-radius: 30rpx;
		
	}
	.content {
		height: calc(100% - 60rpx);
		box-sizing: border-box;
		padding: 30rpx;
		padding-bottom: 188rpx;  // 错过下单组件
	}
	.scrollHeight {
		height: 100%;
		width: 100%;
	}
	.close {
		height: 0;
		bottom: -60rpx;
		transition: all 0.5s;
	}
</style>