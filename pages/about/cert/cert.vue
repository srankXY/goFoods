<template>
	<view>
		<f-goin func-name="实名认证" :content="userInfo.legalName"></f-goin>
		<f-goin func-name="健康证" :content="userInfo.healthCert"></f-goin>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userInfo: {
					id: 1,
					legalName: "未知",
					healthCert: "未知"
				}
			};
		}
	}
</script>

<style lang="scss" scoped>

</style>
