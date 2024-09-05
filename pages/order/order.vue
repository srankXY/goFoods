<template>
	<view>
		<shopCart :foodsArr="testdata" :popup-state="popupState"></shopCart>
		<button type="primary" @tap="openCart">打开购物车</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				popupState: true,
				testdata: [{
					"foodId": 1,
					"foodImg": "http://rused8mwk.hn-bkt.clouddn.com/Fv4GKJ7_nHJW8arie07GV04s-b-D",
					"foodName": "111111",
					"foodPrice": 21,
					"foodCount": 0
				}, {
					"foodId": 2,
					"foodImg": "http://rused8mwk.hn-bkt.clouddn.com/Flr0UQgijM9LwLax_Q6jH6pnE9d3",
					"foodName": "2222",
					"foodPrice": 15,
					"foodCount": 2
				}, {
					"foodId": 3,
					"foodImg": "http://rused8mwk.hn-bkt.clouddn.com/Fv4GKJ7_nHJW8arie07GV04s-b-D",
					"foodName": "111111",
					"foodPrice": 21,
					"foodCount": 0
				}, {
					"foodId": 4,
					"foodImg": "http://rused8mwk.hn-bkt.clouddn.com/Flr0UQgijM9LwLax_Q6jH6pnE9d3",
					"foodName": "2222",
					"foodPrice": 15,
					"foodCount": 2
				}]
			};
		},
		methods: {
			openCart() {
				this.popupState = !this.popupState
			}
		}
	}
</script>

<style lang="scss">

</style>
