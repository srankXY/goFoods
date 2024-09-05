<template>
	<view>
		<f-goin func-name="vip等级" :content="userInfo.vip"></f-goin>
		<f-goin func-name="vip费率" :content="userInfo.vipRate"></f-goin>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userInfo: {
					id: 1,
					vip: "5",
					vipRate: "0.8"
				}
			};
		}
	}
</script>

<style lang="scss" scoped>

</style>
