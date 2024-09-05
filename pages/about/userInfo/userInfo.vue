<template>
	<view>
		<view class="userInfoRow">
			<f-goin func-name="头像" :contentImg="userInfo.avatar" @click.native="testclick"></f-goin>
			<f-goin func-name="昵称" :content="userInfo.nickName"></f-goin>
			<f-goin func-name="食域ID" :content="userInfo.foodIdCard"></f-goin>
			<f-goin func-name="二维码名片" contentImg="/static/user/qrCode.svg" iconContent></f-goin>
		</view>
		<view class="userInfoRow">
			<f-goin func-name="性别" :content="userInfo.gender"></f-goin>
			<f-goin func-name="年龄" :content="userInfo.age"></f-goin>
			<f-goin func-name="电话" :content="userInfo.phone"></f-goin>
			<f-goin func-name="简介" :content="userInfo.signState"></f-goin>
			<f-goin func-name="地区" :content="userInfo.addr"></f-goin>
		</view>
		<view class="userInfoRow">
			<f-goin func-name="地址管理"></f-goin>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userInfo: {
					id: 1,
					avatar: "https://img2.woyaogexing.com/2023/05/21/101dc89b300a15311a6d1afa88327d67.jpg",
					nickName: "肖扬",
					foodIdCard: "000001",
					qrImg: "海报",
					gender: "未知",
					age: "未知",
					phone: "18010591507",
					addr: "未知",
					signState: "这个家伙还没有写自己的简介"
				}
			};
		},
		onLoad() {
			const eventChannel = this.getOpenerEventChannel();
			eventChannel.on('user-id', function(data) {
				console.log(data)
			})
		},
		methods: {
			testclick(){
				console.log(11111111111111111)
			}
		}
	}
</script>

<style lang="scss" scoped>
	.userInfoRow {
		margin-bottom: 20rpx;
	}
</style>
