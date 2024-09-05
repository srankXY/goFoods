<template>
	<view>
		<f-goin func-name="零钱" :content="`¥ `+userInfo.balance"></f-goin>
		<f-goin func-name="收支明细"></f-goin>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userInfo: {
					id: 1,
					balance: 20,
				}
			};
		}
	}
</script>

<style lang="scss" scoped>

</style>
