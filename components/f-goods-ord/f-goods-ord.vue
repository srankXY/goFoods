<template>
	<view class="out">
		<!-- 顶部标题、状态等 -->
		<view class="rowBox">
			<view class="title" @click="jumpToPage(goodsDetailUrl)">
				<image class="foodIcon" src="../../static/funcIcon/about.svg" mode="aspectFill"></image>
				<span>{{title}}</span>
				<image class="foodIcon" src="../../static/user/inRight.svg" mode="aspectFill"></image>
			</view>
			<view class="fontStyle">
				{{goodsState[state]}}
			</view>
		</view>
		<!-- 完成时间，价格等 -->
		<view class="rowBox" @click="jumpToPage(orderDetailUrl)">
			<view class="contenL">
				<image class="goodsImg" :src="img" mode="aspectFill"></image>
			</view>
			<view class="contenM fontStyle">
				<!-- 菜品简介 -->
				<view class="content">
					菜品简介：{{describe}}
				</view>
				<!-- 预约的时间 -->
				<view class="content">
					发布时间：{{time}}
				</view>
			</view>
			<view class="contenR">
				<!-- 商品价格 -->
				￥{{price}}
			</view>
		</view>
		<!-- 删除，类型，“编辑，评价”，再来一单 -->
		<view class="rowBox">
			<span v-if="state==5 || state==0" class="fontStyle" @click="delOrder(gid)">删除</span>
			<view class="btnRight">
				<span class="identiTag" :style="{color: goodsIdentity[type][1]}">
					<image :src="goodsIdentity[type][0]" mode="aspectFill"></image>
					{{typeCn[type]}}
				</span>
				<span class="btn" v-if="state==0" @click="jumpToTab(goodsEditUrl)">编辑</span>
				<span class="btn" v-if="state>2" @click="jumpToPage(orderCommentUrl)">评价</span>
				<span class="btn btnB" @click="tryOrderClick">再来一单</span>
			</view>
		</view>
	</view>
</template>

<script>
	import fGlobal from '../../global.vue';
	export default {
		name:"f-goods-ord",
		props: {
			// 商品id
			gid: {
				type: Number
			},
			ordId: {
				type: Number
			},
			// 商品标题
			title: {
				type: String,
			},
			// 商品状态
			state: {
				type: String
			},
			// 封面
			img: {
				type: String
			},
			// 简介
			describe: {
				type:String
			},
			// 发布时间
			time: {
				type: String
			},
			// 最终价格
			price: {
				type: Number
			},
			// 商品类型
			type: {
				type: String,
				default: 0
			},
		},
		data() {
			return {
				typeCn: fGlobal.goodsType,
				goodsIdentity: fGlobal.goodsIdentity,
				goodsState: fGlobal.goodsState,
				goodsDetailUrl: `/pages/goodsDetails/goodsDetails?gid=${this.gid}&ordId=${this.ordId}`,
				orderDetailUrl: `/pages/order/order?ordId=${this.ordId}`,
				goodsEditUrl: `/pages/push/push?gid=${this.gid}`,
				orderCommentUrl: `/pages/comment/comment?ordId=${this.ordId}`
			};
		},
		methods: {
			jumpToTab(url) {
				uni.reLaunch({
					url: url,
					success: (res) => {
						// res.eventChannel.emit('gid', { gid: this.gid })
					},
				})
			},
			jumpToPage(url) {
				uni.navigateTo({
					url: url,
					success: res => {
						// res.eventChannel.emit('gid', { gid: this.gid })
					},
					fail: () => {},
					complete: () => {}
				});
			},
			delOrder(ordId) {
				// 删除商品goods或者order
				uni.request({
					url: fGlobal.API.baseUrl+fGlobal.API.delOrdPath,
					method: 'POST',
					data: JSON.stringify({
						ordId: ordId
					}),
					success: res => {},
					fail: () => {},
					complete: () => {}
					
				});
			},
			tryOrderClick() {
				if (this.type == 0) {
					// 当goods为用户单时,直接跳转到发布页面,再发布一次
					this.jumpToTab(this.goodsEditUrl)
				}else if (this.type == 1 || this.type == 2) {
					// 当goods为厨师单时,跳转到厨师单详情页面,获取当前订单菜品详情添加至购物车
					// 此处应只做跳转,不做逻辑处理,故下面代码应放到goodsDetail页面中执
					// let ordFoods;
					// uni.request({
					// 	url: fGlobal.API.baseUrl+fGlobal.API.getOrdPath,
					// 	method: 'GET',
					// 	data: {
					// 		ordId: ordId
					// 	},
					// 	success: res => {
					// 		ordFoods = res.data //此处应根据api返回结果进行调整
					// 	},
					// 	fail: () => {},
					// 	complete: () => {}
					// });
					uni.navigateTo({
						// 商品详情页面根据ord id 获取订单菜品信息,再添加至购物车
						url: goodsDetailUrl,
						success: res => {
							// res.eventChannel.emit('gid', { gid: this.gid })
						},
						fail: () => {},
						complete: () => {}
					})
				}
			}
		},
	}
</script>

<style lang="scss" scoped>
	.btnRight {
		width: calc(750rpx - 220rpx);
		display: flex;
		align-items: center;
		justify-content: space-between;
	}
	.btn {
		width: 160rpx;
		height: 55rpx;
		line-height: 55rpx;
		border: 0.5px solid #ccc;
		border-radius: 30rpx;
		text-align: center;
	}
	.btnB {
		color: rgb(202,103,44);
		border: 0.5px solid rgba(202, 103, 44, 0.5);
	}
	.contenM {
		width: calc(750rpx - 330rpx);
	}
	.fontStyle {
		font-size: 28rpx;
		color: #555;
	}
	.identiTag {
		display: flex;
		align-items: center;
	}
	.title,.contenR {
		font-size: 36rpx;
		font-weight: bold;
	}
	.foodIcon {
		width: 40rpx;
		height: 40rpx;
	}
	span {
		// padding: 0 20rpx;
		image {
			width: 35rpx;
			height: 35rpx;
		}
	}
	.goodsImg {
		width: 110rpx;
		height: 110rpx;
	}
	.content {
		word-break: keep-all;       /* 不换行 */
		white-space: nowrap;        /* 不换行 */
		overflow: hidden;           /* 内容超出宽度时隐藏超出部分的内容 */
		text-overflow: ellipsis;	/*溢出时显示省略标记...；需与overflow:hidden;一起使用*/
	}
	.rowBox {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 2rpx 0;
		// margin: 20rpx 0;
	}
	.title {
		display: flex;
		align-items: center;
	}
	.out {
		margin: 0 10rpx;
		margin-bottom: 20rpx;
		border-radius: 20rpx;
		padding-bottom: 15rpx;
	}
</style>