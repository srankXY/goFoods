<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style lang="scss">
	/* 注意要写在第一行，同时给style标签加入lang="scss"属性 */
	@import "@/uni_modules/uview-ui/index.scss";
	page {
		background-color: #f0f0f0;
		font-family: Microsoft JhengHei;
		box-sizing: border-box;
	}
	.ql-editor.ql-blank:before {
	  font-size: 26rpx;
	  color: #909090;
	}
	.ql-editor {
		font-family: Microsoft JhengHei !important;
	}
	.out {
		box-sizing: border-box;
		padding: 30rpx;
		background-color: #fff;
	}
</style>
