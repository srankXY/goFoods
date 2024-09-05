<script>
	// 0:用户,1:师傅,2:商家
	const goodsType = ["用户", "师傅", "商家"];
	const  goodsIdentity = [
		["/static/tag/userTag.svg", "#3C96FF"],
		["/static/tag/personalTag.svg", "#8A2BE2"],
		["/static/tag/teamTag.svg", "#F95A44"],
	];
	const goodsState = [
		'未接单',
		'已接单',
		'进行中',
		'已完成',
		'售后中',
		'已结束',
	];
	
	// api 相关
	const API = {
		baseUrl: "http://127.0.0.1:9999",
		getGoodsPath: "/api/v1/filter",			// 首页获取商品、筛选接口
		pushPath: "/api/v1/publish ",			// 发布接口
		userInfoPath: "/api/v1/userInfo",		// 获取GET、编辑PUT、添加POST用户信息
		placeOrdPath: "/api/v1/placeOrder",		// 下单接口
		delOrdPath: "/deleteGoods", 			// 删除订单
		// getGoodsPath: "/getGoods", 			// 获取商品单子
		getOrdPath: "/getOrders",  				// 获取订单详情
		uploadFilePath: "/api/v1/upload/images", // 上传文件
	};
	
	export default {
		goodsIdentity,
		goodsType,
		goodsState,
		API
	}
</script>