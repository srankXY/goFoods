<template>
	<view>
		<!-- 顶部用户简介 -->
		<f-top :nick-name="userinfo.nickName" :user-id="userinfo.foodIdCard" :avatar="userinfo.avatar"
		:sign-msg="userinfo.signMsg" :health="userinfo.health" :legal="userinfo.legal"></f-top>
		<!-- <f-goods-ord :gid="goods.id" :title="goods.title" :state="goods.state"
		:img="goods.img" :describe="goods.describe" :time="goods.date" :price="goods.price"
		:type="goods.goodsType"></f-goods-ord> -->
		<!-- 内容详情部分 -->
		<view class="detail" v-if="goods.goodsType == 0">
			<!-- 用户单DOM -->
			<view class="title" style="margin-bottom: 40rpx;">
				“{{goods.title}}”
			</view>
			<view class="tag">
				<span>{{userinfo.addrs[goods.addr]}}</span>
				<span>{{goods.foodType[0]}}</span>
				<span>{{goods.foodCount}}个</span>
				<span>{{goods.date}}</span>
				<span>{{goods.time}}</span>
			</view>
			<view class="content">
				<view>- start -</view>
				<rich-text :nodes="goods.foodDetail"></rich-text>
				<view>- end -</view>
			</view>
		</view>
		<!-- 渲染商家页面 -->
		<view class="detail flexStart noPaddingL" v-if="goods.goodsType != 0">
			<!-- 厨师单DOM -->
			<view class="detailLeft">
				<scroll-view scroll-y="true" class="scrollHeight">
					<view v-for="item,idx in goods.foodClasses" :key="idx" @click="classClick(item)">{{item}}</view>
				</scroll-view>
			</view>
			<view class="detailRight">
				<f-food v-for="(val, key) in tmpfoods" :food-id="val.id" :food-img="val.img"
				:food-name="val.name" :food-class="val.class" :food-materjal="val.materjal"
				:food-price="val.price" :food-count="val.foodCount" :key="val.id"
				v-show="val.class == currentClass || currentClass == '全部'"
				@addCart="addCart"></f-food>
			</view>
		</view>
		<view class="footer footerPos">
			<view class="footerLeft flexCenter">
				<view class="cartImg" @tap="openCart" v-if="goods.goodsType!=0">
					<image src="../../static/shopCar.svg" mode="aspectFill"></image>
					<view class="totalFoodCount">{{totalFoodCount}}</view>
				</view>
				
				<!-- 用户单直接显示用户给定的价格 -->
				<span v-if="goods.goodsType==0">￥{{goods.price}}</span>
				<!-- 厨师单根据购物车进行计算 -->
				<span v-if="goods.goodsType!=0">￥{{totalPrice}}</span>
			</view>
			
			<view class="footerRight flexCenter">
				<!-- 厨师接单 -->
				<button v-if="goods.goodsType==0" size="mini" @tap="placeOrd">接单</button>
				<!-- 用户下单 -->
				<button v-if="goods.goodsType!=0" @tap="placeOrd">下单</button>
			</view>
		</view>
		<!-- 购物车组件 -->
		<shopCart :foodsArr="cartList" :popup-state="popupState" @addCart="addCart"></shopCart>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tmpfoods: {},
				popupState: false,
				// 临时购物车，用于同步本地存储中的数据
				tmpCartStorage: {},
				currentClass: "全部",
				cartList: [],
				userinfo: {
					id: 1,
					nickName: "肖扬",
					foodIdCard: "100001",
					avatar: "/static/logo.png",
					signMsg: "今天按时吃饭了吗",
					health: true,
					legal: true,
					addrs: ["武侯区", "双流区", "高新区"]
				},
				goods: {},
				goodsTest1: {
					id: 1,
					ordID: 111,
					title: "今天你还好吗,能做2个菜吗",
					state: "0",
					img: "/static/avatar/avatar-1.png",
					describe: "随便，你开心就好哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈",
					date: "2023-06-02",
					time: "12:00",
					price: 500,
					goodsType: "0",
					foodType: ["川菜"],
					addr: 0,
					foodCount: 5,
					foodDetail: "<h3>阿斯顿发送到</h3><img src=\"https://api.qrtool.cn/?text=https://txttool.cn/editor-tinymce/\">",
				},
				goodsTest2: {
					id: 2,
					title: "武侯区顶级私厨",
					img: "/static/avatar/avatar-1.png",
					describe: "随便，你开心就好哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈",
					date: "2023-06-02",
					price: 500,
					goodsType: "2",
					foodType: ["川菜"],
					addr: 0,
					foodCount: 5,
					foodClasses: ["全部", "炒菜", "凉菜", "饮料", "海鲜", "烧烤", "小吃", "主食"],
					foodDetail: [
						{
							id: 1,
							img:"http://rused8mwk.hn-bkt.clouddn.com/Fv4GKJ7_nHJW8arie07GV04s-b-D",
							name:"111111",
							materjal:"111,2222",
							class:"炒菜",
							price:21,
						},
						{
							id: 2,
							img:"http://rused8mwk.hn-bkt.clouddn.com/Flr0UQgijM9LwLax_Q6jH6pnE9d3",
							name:"2222",
							materjal:"222,333",
							class:"凉菜",
							price:15,
						},
					]
				},
				goodsTest3: {
					id: 3,
					title: "武侯区顶级私厨",
					img: "/static/avatar/avatar-1.png",
					describe: "随便，你开心就好哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈",
					date: "2023-06-02",
					price: 500,
					goodsType: "2",
					foodType: ["川菜"],
					addr: 0,
					foodCount: 5,
					foodClasses: ["全部", "炒菜", "凉菜", "饮料", "海鲜", "烧烤", "小吃", "主食"],
					foodDetail: [
						{
							id: 1,
							img:"http://rused8mwk.hn-bkt.clouddn.com/Fv4GKJ7_nHJW8arie07GV04s-b-D",
							name:"111111",
							materjal:"111,2222",
							class:"炒菜",
							price:21,
						},
						{
							id: 2,
							img:"http://rused8mwk.hn-bkt.clouddn.com/Flr0UQgijM9LwLax_Q6jH6pnE9d3",
							name:"2222",
							materjal:"222,333",
							class:"凉菜",
							price:15,
						},
					]
				},
			};
		},
		onLoad(args) {
			console.log(args)
			// 模拟从网络获取goods信息
			if(args.goodsId == 1) {
				this.goods = this.goodsTest1
			}else if(args.goodsId == 2) {
				this.goods = this.goodsTest2
			}else {
				this.goods = this.goodsTest3
			}
			// 获取商品详情
			uni.uni.request({
				url: fGlobal.API.baseUrl+fGlobal.API.getGoodsPath,
				method: 'GET',
				data: JSON.stringify(args),
				success: res => {
					this.goods = res.data
					
				},
				fail: () => {},
				complete: () => {}
			});
			
			// 如果为商家goods则执行以下代码,否则不执行
			if(this.goods.goodsType != "0") {
				// 从本地存储中获取购物车数据,并赋值给临时购物车tmpCartStorage
				uni.getStorage({
					key: 'cartStorage',
					success: (res) => {
						this.tmpCartStorage = JSON.parse(res.data)
						
						// 根据goodsid 获取该店铺的购物车数据，线上更换1为args.gid
						if (this.tmpCartStorage[args.goodsId]) {
							// 该店铺购物车中有数据才执行下面代码
							this.cartList = this.tmpCartStorage[args.goodsId]
						}
					},
				})
				// 重新渲染商店接口返回的菜品数据存到tmpfoods对象中,添加foodCount方便后期购物车数据响应
				this.goods.foodDetail.forEach((foodItem, foodIdx) => {
					// 重组店铺菜品对象
					this.tmpfoods[foodItem.id] = foodItem
					this.cartList.forEach((cartItem) => {
						if (cartItem.foodId == foodItem.id) {
							this.tmpfoods[foodItem.id].foodCount = cartItem.foodCount
						}
					})
				})
			}
			
			// console.log(this.cartList)
			// console.log(this.tmpfoods)
			// console.log(args)
			// 使用event方式传递参数
			// const eventChannel = this.getOpenerEventChannel();
			// eventChannel.on('gid', function(data) {
			// 	console.log(data)
			// })
		},
		computed: {
			// 店铺购物车金额统计
			totalPrice() {
				let price = 0
				this.cartList.forEach((item) => {
					price = (item.foodCount * item.foodPrice) + price
				})
				return price
			},
			totalFoodCount() {
				return this.cartList.length
			}
		},
		methods: {
			openCart() {
				this.popupState = !this.popupState
			},
			placeOrd(){
				uni.uni.request({
					url: fGlobal.API.baseUrl+fGlobal.API.placeOrdPath,
					method: 'POST',
					data: JSON.stringify({
						goodsId: this.goods.id,
						foods: this.tmpCartStorage[this.goods.id]
					}),
					success: res => {
						
					},
					fail: () => {},
					complete: () => {}
				});
			},
			classClick(v) {
				// 选择菜品分类，根据当前分类渲染显示的菜品数据
				this.currentClass = v
			},
			addCart(foodItem) {
				// 更新店铺tmpfoods菜品obj中的菜品数量信息
				this.tmpfoods[foodItem.foodId].foodCount = foodItem.foodCount
				// foodItem为f-food组件传递出来的反馈值
				// 记录该菜品是否存在于购物车中
				let isExist = false
				this.cartList.forEach((cartItem, cartIdx) => {
					if (cartItem.foodId == foodItem.foodId) {
						// 存在则修改状态为true，并替换原有数据
						isExist = true
						this.cartList.splice(cartIdx, 1, foodItem)
						
						// 如果菜品数量为0，则删除购物车中相关菜品数据
						if (foodItem.foodCount == 0) {
							this.cartList.splice(cartIdx, 1)
						}
					}
					
				})
				if (!isExist) {
					//不存在则直接添加元素到店铺购物车
					this.cartList.push(foodItem)
				}
				// 更新当前goods id的店铺的购物车信息到临时大购物车变量中
				this.tmpCartStorage[this.goods.id] = this.cartList
				uni.setStorage({
					// 把临时大购物车中的数据存储到用户本地
					key: 'cartStorage',
					data: JSON.stringify(this.tmpCartStorage),
					success: () => {
						console.log("购物车数据本地存储完成");
					}
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.cartImg {
		position: relative;
		width: 100rpx;
		height: 100rpx;
		.totalFoodCount {
			position: absolute;
			right: 0;
			top: 0;
			color: #fff;
			width: 35rpx;
			height: 35rpx;
			line-height: 35rpx;
			border-radius: 50%;
			background-color: #f95a44;
			text-align: center;
			font-size: 26rpx;
		}
		image {
			width: 100%;
			height: 100%;
		}
	}
	.footerPos {
		position: fixed;
		bottom: 50rpx;
		// height: 100%;
		width: 100%;
		z-index: 9999;
	}
	
	.detailRight {
		flex: 1;
	}
	.flexStart {
		display: flex;
		justify-content: space-between;
	}
	
	.detailLeft {
		// border: 1px solid #000;
		width: 160rpx;
		text-align: center;
		background-color: #f9f9f9;
	}
	.detail {
		background-color: #fff;
		box-sizing: border-box;
		padding: 20rpx;
		margin-bottom: 20rpx;
		view {
			margin: 30rpx 0;
		}
	}
	.title {
		font-size: 40rpx;
		font-weight: bold;
		text-align: center;
	}
	.tag {
		display: flex;
		align-items: center;
		justify-content: space-between;
		box-sizing: border-box;
		padding: 0 30rpx;
		span {
			background-color: #eee;
			padding: 0rpx 10rpx;
		}
	}
	.content {
		text-align: center;
		padding-top: 10rpx;
		view {
			color: #aaa;
			margin-top: 0;
		}
		img {
			width: 350rpx;
			height: 350rpx;
		}
	}
	.footer {
		background-color: #fff;
		display: flex;
		align-items: center;
		justify-content: space-between;
		box-sizing: border-box;
		padding: 0 60rpx;
		span {
			font-size: 45rpx;
			font-weight: bold;
			color: #fb8106;
		}
		button {
			font-size: 40rpx;
			color: limegreen;
			border-radius: 20rpx;
			border: 1px solid rgba(50,205,50,.4);
			padding: 0 80rpx;
			margin: 20rpx 0;
		}
	}
	.flexCenter {
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.noPaddingL {
		padding-left: 0;
	}
	.scrollHeight {
		// height: 100rpx;
		view {
			height: 120rpx;
			line-height: 120rpx;
			// border: 1px solid #000;
			margin: 0;
		}
		view:hover {
			background-color: #fff;
		}
	}
</style>
