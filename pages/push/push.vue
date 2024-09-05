<template>
	<view class="push">
		<view class="rowBox">
			<view class="rowTop">
				<span>标题:</span>
				<input type="text" placeholder="请输入标题(不超过15个字)" placeholder-class="placeholder-class" maxlength=15 v-model="pushData.title">
			</view>
			<!-- <span>{{notice.title}}</span> -->
		</view>
		<!-- <view class="rowBox">
			<view class="rowTop">
				<span>订单类型:</span>
				<picker @change="goodsTypePicker" :value="goodsTypeIdx" :range="goodsType">
					<view class="uni-input">{{goodsType[goodsTypeIdx]}}</view>
				</picker>
			</view>
		</view> -->
		<view class="rowBox">
			<view class="rowTop">
				<span>菜系:</span>
				<scroll-view class="scroll-view_H" scroll-x>
					<checkbox-group class="checkBox" name="foodTypeSelect" @change="foodTypeSelect">
						<label v-for="item, idx in foodType" :key="idx">
							<checkbox :value="item" :checked="pushData.foodType.indexOf(item)>-1 ? true : false" /><text>{{item}}</text>
						</label>
					</checkbox-group>
				</scroll-view>
			</view>
			<!-- <span>{{notice.title}}</span> -->
			<!-- <uni-data-checkbox multiple v-model="pushData.foodType" :localdata="foodType" @change="change"></uni-data-checkbox> -->
		</view>
		<view class="rowBox">
			<view class="rowTop">
				<span>数量:</span>
				<u-number-box v-model="pushData.foodCount" class="numberBox"></u-number-box>
				<span>价格:</span>
				<u-number-box v-model="pushData.price" class="numberBox"></u-number-box>
			</view>
			<!-- <span>{{notice.title}}</span> -->
			<!-- <u-slider v-model="pushData.foodCount" showValue blockSize=22 ></u-slider> -->
		</view>
			
		<view class="rowBox">
			
			<view class="rowTop">
				<span>时间:</span>
				<picker mode="date" :value="pushData.date" :start="startDate" :end="endDate" @change="bindDateChange">
					<view class="date">{{pushData.date}}</view>
				</picker>
				<picker mode="time" :value="pushData.time" start="09:01" end="22:01" @change="bindTimeChange">
					<view class="uni-input">{{pushData.time}}</view>
				</picker>
			</view>
			<!-- <span>{{notice.title}}</span> -->
			
		</view>
		<view class="rowBox">
			<view class="rowTop">
				<span>地址:</span>
				<picker @change="addrPicker" :value="addrIdx" :range="userInfo.addrs">
					<view class="uni-input">{{userInfo.addrs[addrIdx]}}</view>
				</picker>
			</view>
			<!-- <span>{{notice.title}}</span> -->
		</view>
		<view class="rowBox">
			<view class="rowTop">
				<span>描述:</span>
				<input type="text" placeholder="首页展示简介" placeholder-class="placeholder-class" v-model="pushData.describe">
			<!-- <textarea v-model="pushData.describe" cols="30" rows="3"></textarea> -->
			</view>
			<!-- <span>{{notice.title}}</span> -->
		</view>
		<view class="rowBox">
			<view class="rowTop idxImg">
				<span>订单类型:</span>
				<picker @change="goodsTypePicker" :value="goodsTypeIdx" :range="goodsType">
					<view class="uni-input">{{goodsType[goodsTypeIdx]}}</view>
				</picker>
				<span>封面:</span>
				<view class="foodLeft" @click="addFoodPicUrl('idxImg')">
					<image :src="pushData.img" mode="aspectFill"></image>
				</view>
			</view>
			<!-- <span>{{notice.title}}</span> -->
		</view>
		<view class="rowBox">
			<view class="rowTop">
				<span style="height:0;">菜品详情:</span>
			</view>
			<jinEdit placeholder="请输入" ref="editot" :uploadFileUrl="uploadFileUrl" height="600rpx" @editorBlur="bindFoodDetail" v-show="pushData.goodsType==0"></jinEdit> 
			<!-- <textarea v-model="pushData.foodDetail" cols="30" rows="3" v-if="pushData.goodsType=='用户'"></textarea> -->
			<view v-if="pushData.goodsType!=0">
				<view class="addFood" v-for="item in foodCount" :key="item">
					<view class="foodLeft" @click="addFoodPicUrl(item-1)">
						<image :src="tmpFoodPic[item-1]" mode="aspectFill"></image>
					</view>
					<view class="foodRight">
						<view class="foodName">
							<input type="text" placeholder="菜品名称" placeholder-class="placeholder-class" @blur="saveFoodDetail($event, 'name', item-1)">
						</view>
						<view class="foodMaterjal">
							<input type="text" placeholder="菜品配料" placeholder-class="placeholder-class" @blur="saveFoodDetail($event, 'materjal', item-1)">
						</view>
						<view class="foodClass">
							<input type="text" placeholder="菜品分类" placeholder-class="placeholder-class" @blur="saveFoodDetail($event, 'class', item-1)">
						</view>
						<view class="foodPrice">
							<span>价格:</span>
							<u-number-box @change="saveFoodDetail($event, 'price', item-1)" class="numberBox"></u-number-box>
						</view>
					</view>
				</view>
				<button @click="clickAddFood">添加菜品</button>
			</view>
		</view>
		
		<view
		:class="(pageHeight - scrollTop) > 900 ? 'posHidden' : 'posShow'"
		>
			<view class="button">
				<image src="../../static/push/reset.svg" mode="aspectFill" @click="resetClick"></image>
			</view>
			<view class="button">
				<image src="../../static/push/submit.svg" mode="aspectFill" @click="phshClick"></image>
			</view>
		</view>
	</view>
</template>

<script>
	import jinEdit from '../../components/jin-edit/jin-edit.vue';
	import { created } from '../../uni_modules/uview-ui/libs/mixin/mixin';
	import fGlobal from '../../global.vue';
	export default {
		components: {
			jinEdit
		},
		data() {
			
			return {
				uploadFileUrl: fGlobal.API.baseUrl+fGlobal.API.uploadFilePath,
				foodCount: 1,
				// 厨师上传菜品详情图片临时存储,用于页面图片实时展示
				tmpFoodPic: [],
				scrollTop:0,
				// foodType: [{"value": 0,"text": "川菜"	},{"value": 1,"text": "粤菜"},{"value": 2,"text": "鲁菜"},{"value": 3,"text": "湘菜"},{"value": 4,"text": "徽菜"},{"value": 5,"text": "浙菜"}],
				foodType: ["川菜", "粤菜", "鲁菜", "湘菜", "徽菜", "浙菜"],
				goodsType: fGlobal.goodsType,
				goodsTypeIdx: 0,
				addrIdx: 0,
				userInfo: {
					id: 0,
					addrs: ["成都市武侯区燃灯寺西北街36号院6-2", "成都市双流区华阳嘉美地3-1006", "成都市高新区融城后街2-1008"],
				},
				pushData: {},
				notice: {
					title: null,
					foodType: null
				},
			};
		},
		onPageScroll(e) {
				this.scrollTop = e.scrollTop;
				// console.log(this.pageHeight,this.scrollTop,this.pageHeight-this.scrollTop)
		},
		
		onLoad(args) {
			// 如果没有传递参数到该页面则为新发布，设置默认数据
			if (Object.keys(args).length === 0) {
				this.pushData = this.defaultData()
			}else {
				// 接受一个goods id,通过id 获取原有数据赋值给pushData,从而修改原有goods
				// 需注意编辑功能和再来一单功能，未成交的订单为更新订单，已经成交的订单为新发布一个订单
				uni.request({
					url: fGlobal.API.baseUrl+fGlobal.API.getGoodsPath,
					method: 'GET',
					data: args,
					success: res => {
						// 把获取到的数据重新赋值给pushData
						this.pushData = res.data
					},
					fail: () => {},
					complete: () => {}
				});
				// 渲染获取到的非v-model数据到页面
				// .....
				// <coding/>
			}
						
						// this.postData()
		},
		computed: {
			pageHeight(){
				return uni.getSystemInfoSync().screenHeight
			},
			startDate() {
				return this.getDate('start');
			},
			endDate() {
				return this.getDate('end');
			},
		},
		methods: {
			clickAddFood(){
				this.foodCount++
			},
			// 厨师上传菜品详情
			saveFoodDetail(e, n, i) {
				// console.log(e)
				let food = {}
				if (this.pushData.foodDetail[i] != null) {
					food = this.pushData.foodDetail[i]
				}
				if (n == 'img') {
					food[n] = e
				}else if (n == 'price') {
					food[n] = e.value
				}else {
					food[n] = e.detail.value
				}
				this.pushData.foodDetail[i] = food
				// console.log(this.pushData)
			},
			// 厨师上传菜品详情图片
			addFoodPicUrl(i) {
				uni.chooseImage({
					count: 1, //定义上传的图片数量
					success: res=>{
						uni.showLoading({
							title: '正在上传中...'
						})
						uni.uploadFile({
							url: fGlobal.API.baseUrl+fGlobal.API.uploadFilePath,
							filePath: res.tempFiles[0].path,
							success: res => {
								// 如果参数为idxImg, 则说明上传的封面图片
								if (i == 'idxImg') {
									this.pushData.img = JSON.parse(res.data).url
								} else {
									// 厨师上传的菜品详情图片
									this.$set(this.tmpFoodPic, i, JSON.parse(res.data).url)
									this.saveFoodDetail(this.tmpFoodPic[i], 'img', i)
								}
								uni.hideLoading()
							},
						});
					}
				})
			},
			postData(){
				uni.request({
					url: fGlobal.API.baseUrl+fGlobal.API.pushPath,
					data: JSON.stringify(this.pushData),
					method: 'POST',
					success: (res) => {
						console.log(res)
					}
				})
			},
			defaultData() {
				const date = new Date();
				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();
				return {
					title: null,
					img: null,
					foodType: ["川菜","粤菜"],
					goodsType: 0,
					foodCount: 1,
					date: `${year}-${month}-${day}`,
					time: "12:00",
					price: 0,
					addr: this.addrIdx,
					describe: null,
					foodDetail: null,
				}
			},
			bindFoodDetail(e){
				this.pushData.foodDetail = e.detail.html
				// console.log(this.pushData)
			},
			addrPicker(e){
				this.addrIdx = e.detail.value;
				this.pushData.addr = this.addrIdx
				// this.pushData.addr = this.userInfo.addrs[this.addrIdx]
			},
			bindDateChange(e) {
				// console.log(e);
				this.pushData.date = e.detail.value
			},
			bindTimeChange(e) {
				// console.log(e);
				this.pushData.time = e.detail.value
			},
			foodTypeSelect(e) {
				this.pushData.foodType = e.detail.value;
				// console.log(e);
				// console.log(this.pushData)
			},
			phshClick() {
				console.log(`json_data: ${JSON.stringify(this.pushData)}`)
				this.postData()
			},
			resetClick(){
				this.pushData = this.defaultData()
				this.goodsTypeIdx = 0
				this.addrIdx = 0
				this.$refs.editot.clearAll()
				this.tempFiles = []
				console.log(`json_data:${JSON.stringify(this.pushData)}`)
			},
			goodsTypePicker(e) {
				this.goodsTypeIdx = e.detail.value;
				this.pushData.goodsType = this.goodsTypeIdx;
				// 切换订单类型则把菜品详情置空,但不清除富文本编辑器
				this.pushData.foodDetail = []
				// console.log(e.detail.value);
				
			},
			getDate(type) {
				const date = new Date();
				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();
	
				if (type === 'start') {
					year = year - 60;
				} else if (type === 'end') {
					year = year + 2;
				}
				month = month > 9 ? month : '0' + month;
				day = day > 9 ? day : '0' + day;
				return `${year}-${month}-${day}`;
			}
		}
	}
</script>

<style lang="scss" scoped>
	.foodLeft {
		width: 260rpx;
		height: 260rpx;
		display: flex;
		align-items: center;
		justify-content: center;
		background-color: #e9e9e9;
		background-image: url("../../static/push/choose.svg");
		background-size: 100rpx 100rpx;
		background-repeat: no-repeat;
		background-position: center;
		border-radius: 20rpx;
		image {
			width: 100%;
			height: 100%;
		}
	}
	.addFood {
		box-sizing: border-box;
		padding: 0 30rpx;
		// border: 1px solid #000;
		height: 300rpx;
		display: flex;
		align-items: center;
		justify-content: space-between;
		
		.foodRight {
			box-sizing: border-box;
			padding-left: 30rpx;
			height: 260rpx;
			width: calc(100% - 260rpx);
			display: flex;
			flex-direction: column;
			justify-content: space-between;
			.foodName {
				// border: 1px solid #000;
			}
			.foodPrice {
				display: flex;
				align-items: center;
				// justify-content: center;
				font-size: 32rpx;
				span {
					margin-right: 60rpx;
				}
			}
		}
	}
	// .push {
	// 	width: 750rpx;
	// }
	// button {
	// 	width: 350rpx;
	// }
	.posBottom {
		position: fixed;
		right: 20rpx;
		bottom: calc( var(--window-bottom) + 100px);
		z-index: 1030;
		margin-bottom: 6;
	}
	.placeholder-class {
		font-size: 26rpx;
		// font-family: Microsoft JhengHei;
		color: #909090;
		line-height: 40rpx;
	}
	.rowBox {
		margin: 20rpx 0;
		background-color: #fff;
		border-radius: 10rpx;
		
	}
	.posShow {
		position: fixed;
		right: 40rpx;
		bottom: 240rpx;
		opacity: 1;
		transition: all 1s;
	}
	.posHidden {
		position: fixed;
		right: 40rpx;
		bottom: 240rpx;
		opacity: 0;
		transition: all 1s;
	}
	.button {
		width: 100rpx;
		height: 100rpx;
		// border: 1px #000 solid;
		background-color: rgb(225, 225, 225);
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		margin-bottom: 20rpx;
		image {
			width: 40rpx;
			height: 40rpx;
		}
	}
	.button:active {
		transform: scale(0.9);
	}
	textarea {
		border: 1px rgba(0, 0, 0, .8) solid;
		box-sizing: border-box;
		border-radius: 10rpx;
		height: 300rpx;
		padding: 20rpx;
		width: 95%;
		margin: 0 auto;
	}
	.rowTop {
		display: flex;
		align-items: center;
		height: 100rpx;
		
		.scroll-view_H {
			padding-left: 30rpx;
			white-space: nowrap;
			width: 80%;
			checkbox-group {
				
				checkbox {
					padding: 0 20rpx;
					display: inline-block;;
				}
			}
		}
		span {
			padding: 0 20rpx;
			// width: 100rpx;
			margin: 0 10rpx;
		}
		
		.numberBox {
			margin: 0 auto;
		}
		
		input {
			flex: 1;
			// border: 1px rgba(0, 0, 0, .8) solid;
			border-radius: 10rpx;
			height: 70rpx;
			padding: 0 20rpx;
			margin: 0 auto;
		}
		
	}
	picker {
		flex: 1;
		margin: 0 auto;
		height: 70rpx;
		// border: 1px rgba(0, 0, 0, .8) solid;
		line-height: 70rpx;
		text-align: center;
	}
	.idxImg {
		height: 180rpx;
		.foodLeft {
			width: 150rpx;
			height: 150rpx;
			background-size: 60rpx 60rpx;
			margin: 0 53rpx;
		}
		// picker {
		// 	margin-right: 60rpx;
		// }
	}
	
	
	// .rowBox {
	// 	padding: 0 10rpx;
	// 	margin: 10rpx 0;
	// 	span {
	// 		color: red;
	// 	}
	// }
	// input {
	// 	border: 1px rgba(0, 0, 0, .2) solid;
	// 	border-radius: 5rpx;
	// 	height: 70rpx;
	// 	margin-top: 10rpx;
	// }
	// .test {
	// 	width: 200rpx;
	// 	height: 200rpx;
	// 	background-color: white;
	// }
</style>
