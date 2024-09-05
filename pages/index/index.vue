<template class="all">
	<view class="content">
		<view class="searchView">
			<u-search borderColor="rgba(0,0,0,0.1)" placeholder="请输入搜索内容" v-model="keyword" actionText="搜索" @custom="search"></u-search>
		</view>
		<!-- <image src="/static/banner/banner-1.jpg" mode=""></image> -->
		<view class="swiperView">
			<u-swiper
				:list="list"
				showTitle
				keyName="image"
				previousMargin="30"
				nextMargin="30"
				circular
				radius="5"
				bgColor="#ffffff"
				height="300rpx"
				class="swiper"
			></u-swiper>
		</view>
		<view class="notifyView">
			<u-notice-bar :text="notifyText" mode="closable"></u-notice-bar>
		</view>
		<view class="selectView">
			<sl-filter :themeColor="filterColor" :menuList="filterList" @result="filterClick"></sl-filter>
		</view>
		
		<view class="switchBox">
			<view class="textView">
				<span :style="textType==0 ? 'background-color: rgb(241,243,244)' : ''"><u--text text="进行中" size="30rpx" color="rgba(0,0,0,0.5)" @click="textTypeClick(0)"></u--text></span>
				<span :style="textType==1 ? 'background-color: rgb(241,243,244)' : ''"><u--text text="已结束" size="30rpx" color="rgba(0,0,0,0.5)" @click="textTypeClick(1)"></u--text></span>
			</view>
			<view class="switchView">
				<view>
					<switch :checked="switchBox[0]" color="#c0c0c0" style="transform:scale(0.7)" @click="switchClick([0])" />看用户
				</view>
				<view>
					<switch :checked="switchBox[2]" color="#c0c0c0" style="transform:scale(0.7)" @click="switchClick([1,2])" />看师傅
				</view>
			</view>
			
		</view>
		<!-- 渲染进行中和已结束的DOM体 -->
		<view v-if="textType==0">
			<view class="goodsRow" v-for="item in goods" :key="item.id" v-show="switchBox[item.goodsType]"
			@tap="jumpToPage(item.id)">
				<view class="goodsLeft">
					<view class="goodsImg">
						<image :src="item.img" mode="aspectFill"></image>
					</view>
				</view>
				<view class="goodsRight">
					<view class="goodsTitle">
						{{item.title}}
					</view>
					<view class="goodsTag">
						<span class="noMarginLeft">{{item.foodType}}</span>
						<span>{{item.foodCount}}个</span>
						<span>{{item.time}}</span>
						<span class="goodsPrice">¥{{item.price}}</span>
					</view>
					<view class="describe">
						<text>菜品详情：{{item.describe}}</text>
					</view>
					<view class="goodsBottom">
						<view>
							<span class="noMarginLeft">{{item.addr}}</span>
							<span>{{item.pusher}}</span>
							<span class="goodsIdentity" :style="'color:'+goodsIdentity[item.goodsType][1]"><image :src="goodsIdentity[item.goodsType][0]"></image>{{typeCn[item.goodsType]}}</span>
						</view>
						<button class="mini-btn" type="default" size="mini">{{goodsState[textType]}}</button>
					</view>
				</view>
			</view>
		</view>
		<view v-else-if="textType==1">
			<view class="goodsRow" v-for="item in expiredGoods" :key="item.id" v-show="switchBox[item.goodsType]"
			@tap="jumpToPage(item.id)">
				<view class="goodsLeft">
					<view class="goodsImg">
						<image :src="item.img" mode="aspectFill"></image>
					</view>
				</view>
				<view class="goodsRight">
					<view class="goodsTitle">
						{{item.title}}
					</view>
					<view class="goodsTag">
						<span class="noMarginLeft">{{item.foodType}}</span>
						<span>{{item.foodCount}}个</span>
						<span>{{item.time}}</span>
						<span class="goodsPrice">¥{{item.price}}</span>
					</view>
					<view class="describe">
						<u--text :lines="2" :text="'菜品详情：'+item.describe" size="15rpx"></u--text>
					</view>
					<view class="goodsBottom">
						<view>
							<span class="noMarginLeft">{{item.addr}}</span>
							<span>{{item.pusher}}</span>
							<span class="goodsIdentity" :style="'color:'+goodsIdentity[item.goodsType][1]"><image :src="goodsIdentity[item.goodsType][0]"></image>{{typeCn[item.goodsType]}}</span>
						</view>
						<button class="mini-btn" disabled type="default" size="mini">{{goodsState[textType]}}</button>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
import slFilter from '@/components/sl-filter/sl-filter.vue';
import search from '../../uni_modules/uview-ui/libs/config/props/search';
import { methods } from '../../uni_modules/uview-ui/libs/mixin/mixin';
import fGlobal from '../../global.vue'
	export default {
		components: {
			slFilter
		},
		data() {
			return {
				textType: 0,
				keyword: "私厨肖扬",
				switchBox: [true, true, true],
				typeCn: fGlobal.goodsType,
				// F9AE3D
				goodsIdentity: fGlobal.goodsIdentity,
				goodsState: ["立即沟通", "已结束"],
				expiredGoods: [
					{
						id: 4,
						img: "/static/avatar/avatar-4.png",
						title: "麓山大道202别墅区周末找师傅",
						goodsType: 2,
						addr: "天府新区",
						pusher: "李先生",
						foodType: "川菜",
						foodCount: 20,
						price: 1200,
						time: "2023.3.1",
						foodDetail: "无具体要求，随意发挥，好吃就行"
					},
				],
				
				goods:[
					{
						id: 1,
						img: "/static/avatar/avatar-1.png",
						title: "周五需要上门做饭，5个菜",
						goodsType: 0,
						addr: "武侯区",
						pusher: "肖先生",
						foodType: "川菜",
						foodCount: 5,
						price: 300,
						time: "2023.5.1",
						foodDetail: "紫菜蛋花汤，麻婆豆腐，酸菜鱼，干煸豇豆，白灼大虾"
					},
					{
						id: 2,
						img: "/static/avatar/avatar-3.png",
						title: "老规矩天府新区5个菜",
						goodsType: 1,
						addr: "天府新区",
						pusher: "肖先生",
						foodType: "川菜",
						foodCount: 5,
						price: 300,
						time: "2023.5.6",
						foodDetail: "紫菜蛋花汤，麻婆豆腐，酸菜鱼，干煸豇豆，白灼大虾"
					},
					{
						id: 3,
						img: "/static/avatar/avatar-2.png",
						title: "双流区5.1需要上门做菜",
						goodsType: 2,
						addr: "航空港",
						pusher: "陈女士",
						foodType: "川菜",
						foodCount: 10,
						price: 500,
						time: "2023.5.1",
						foodDetail: "不限"
					},
				],
				filterColor: "#000000",
				notifyText: "今日特惠：下单即送200RMB限时优惠券",
				list: [
					{
						image: '/static/banner/banner-1.jpg',
						title: '顶尖厨宴，让你体会极致快乐'
					},
					{
						image: '/static/banner/banner-2.jpg',
						title: '欢乐时光，食域保障'
					},
					{
						image: '/static/banner/banner-3.jpg',
						title: '食域私厨，让你健康生活'
					}
				],
				filterList: [
					{
						'title': '价格',
						'detailTitle': '请选择价格区间（可多选）',
						'isMutiple': true,
						'key': 'priceSpan',
						'detailList': [{
							'title': '不限',
							'value': ''
						},
						{
							'title': '0-200',
							'value': '0-200'
						},
						{
							'title': '200-500',
							'value': '200-500'
						},
						]
					},
					{
						'title': '距离',
						'detailTitle': '请选择接单范围（单选）',
						'isMutiple': false,
						'key': 'area',
						'detailList': [{
							'title': '不限',
							'value': ''
						},
						{
							'title': '1km以内',
							'value': '<1km'
						}
						]
					},
					{
						'title': '时间',
						'detailTitle': '请选择时间',
						'isMutiple': false,
						'key': 'timeArea',
						'detailList': [{
							'title': '不限',
							'value': ''
						},
						{
							'title': '1周以内',
							'value': '<1week'
						}
						]
					},
					{
						'title': '排序',
						'detailTitle': '请选择排序方式',
						'isSort': true,
						'key': 'sortMethod',
						'detailList': [{
							'title': '默认排序',
							'value': ''
						},
						{
							'title': '发布时间',
							'value': 'add_time'
						},
						{
							'title': '价格最高',
							'value': 'wages_up'
						},
						{
							'title': '离我最近',
							'value': 'location'
						}]
					},
				],
			};
		},
		onLoad() {
			this.getGoods()
		},
		methods:{
			jumpToPage(goodsId) {
				uni.navigateTo({
					url: `/pages/goodsDetails/goodsDetails?goodsId=${goodsId}`,
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			},
			getGoods(data){
				uni.request({
					url: fGlobal.API.baseUrl+fGlobal.API.getGoodsPath,
					data: data,
					method: 'GET',
					success: (res) => {
						console.log(res.data.data)
						this.goods = res.data.data
					}
				})
			},
			search(){
				this.keyword="私厨新奇"
			},
			filterClick(val) {
				console.log('filter_result:' + JSON.stringify(val));
				// let filterResult = JSON.stringify(val, null, 2)
				this.getGoods(val)
				
			},
			textTypeClick(v) {
				this.textType=v;
				console.log(this.textType)
			},
			switchClick(arr) {
				// console.log(this.switchBox['user']);
				for (let i of arr) {
					this.$set(this.switchBox, i, !this.switchBox[i])
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	.goodsRow {
		border-radius: 40rpx;
		background-color: #fff;
		margin: 20rpx 20rpx;
		display: flex;
		// height: 240rpx;
		font-family: Microsoft JhengHei;
		.goodsLeft {			
			.goodsImg {
				image {
					width: 240rpx;
					height: 240rpx;
				}
			}
		}
		.goodsRight {
			width: 510rpx;
			margin-left: 10rpx;
			// display: flex;
			// flex-direction: column;
			// align-items: flex-start;
			.goodsTitle {
				font-weight: bold;
			}
			
			.goodsTag {
				text-align-last:left;
				margin: 5rpx 0;
				.noMarginLeft {
					margin-left: 0;
				}
				.goodsPrice {
					background-color: #fff;
					font-size: 38rpx;
					margin: 0;
					font-family: SimHei;
					color: rgb(251,129,6);
					// padding: 0;
				}
				span {
					font-size: 15rpx;
					margin: 0 15rpx;
					padding: 0 15rpx;
					border-radius: 5rpx;
					background-color: rgb(240,240,240);
				}
				
			}
			.describe {
				text {
					font-size: 15rpx;
					overflow:hidden; 
					text-overflow:ellipsis;
					display:-webkit-box; 
					-webkit-box-orient:vertical;
					-webkit-line-clamp:2;
				}
			}
			
			.goodsBottom {
				display: flex;
				margin-bottom: 5rpx;
				align-items: center;
				.noMarginLeft {
					margin-left: 0;
				}
				view {
					// height: 54rpx;
					width: 290rpx;
					margin-top: 0;
					display: flex;
					justify-content: space-between;
					.goodsIdentity {
						background-color: #fff;
						display: flex;
						align-items: center;
						// border: 1px #000 solid;
						image {
							width: 33rpx;
							height: 33rpx;
						}
					}
					span {
						font-size: 15rpx;
						margin: 0 5rpx;
						padding: 0 5rpx;
						border-radius: 5rpx;
						background-color: rgb(240,240,240);
					}
				}
				button {
					border: rgba(0,0,0,0.3) 1px solid;
					font-size: 18rpx;
					// margin-left: 0 10rpx;
				}
				
			}
		}
	}
	.switchBox {
		width: 90%;
		margin: 0 auto;
		display: flex;
		align-items: center;
		.switchView {
			margin-top: 0;
			font-size: 28rpx;
		}
		.textView {
			margin-top: 0;
			display: flex;
			padding: 0 30rpx;
			span {
				border-radius: 30rpx;
				background-color: #fff;
				border: 1px rgba(0,0,0,0.2) solid;
				line-height: 70rpx;
				padding: 0 30rpx;
				margin-left: 20rpx;
				margin-right: 30rpx;
			}
		}
	}
	
	.test {
		font-family: iconfont;
		font-size: 60rpx;
		
	}
	
	.searchView{
		margin: 0 auto;
		padding: 0 40rpx;
		background-color: #fff;
	}
	.swiperView {
		background-color: #fff;
	}
	
	.notifyView{
		width: 100%;
		.u-notice-bar{
			width: 90%;
			margin: 0 auto;
		}
	}
	
	.content{
		width: 750rpx;
		view {
			margin-top: 20rpx;
		}
	}
	
</style>
