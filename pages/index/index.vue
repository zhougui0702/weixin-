<template>
	<view class="con-box">
		<view class="con-box-ziyuan">
			<view class="con-box-ziyuan-top">
				<view class="con-box-ziyuan-top-name">精选推荐</view>
				<view class="con-box-ziyuan-top-desc">（精品资源优先推荐，您值得拥有）</view>
			</view>
			<view class="con-box-ziyuan-bottom">
				<block v-for="(item, index) in result_list">
					<view @tap="toDetail" :data-id="item['id']" :data-type="item['type']" :data-tt_appid="item['tt_appid']" :data-wx_appid="item['wx_appid']" :data-extend="item['extend']" class="con-box-ziyuan-list">
						<view class="con-box-ziyuan-list-image">
							<image mode="scaleToFill" :src="item['image']"></image>
						</view>
						<view class="con-box-ziyuan-list-title">{{item['store_name']}}</view>
						<view class="con-box-ziyuan-list-bottom">
							<view class="con-box-ziyuan-list-bottom-num">{{item['click']}}人查看</view>
							<view class="con-box-ziyuan-list-bottom-num">{{item['download_number']}}人想要</view>
						</view>
					</view>
				</block>
			</view>
		</view>
		<view style="height: 80rpx;"></view>
	</view>
</template>	

<script>
	
	const app = getApp();
	export default {
		data() {
			return {
				result_list:[],
			};
		}
		/**
		 * 生命周期函数--监听页面加载
		 */
		,
		onLoad: function(options) {
			var that = this;
			uni.showLoading({
			  title: '加载中',
			})
			//获取接口数据---接口不同可联系微信【dongli3023】
			uni.request({
				url: 'https://ziyuan.03023.net/api/index',
				data: {
				},
				method: 'GET',
				header: {
					'Content-Type': 'application/x-www-form-urlencoded',
				},
				success: res => {
				    uni.hideLoading();
				    var result_list = res.data['data']['list']['data'];
					//console.log(res);
					that.result_list = result_list;
				},
				fail: function (res) {
					console.log(res);
				}
			});
		},
		/**
		 * 生命周期函数--监听页面初次渲染完成
		 */
		onReady: function() {},
		/**
		 * 生命周期函数--监听页面显示
		 */
		onShow: function() {},
		/**
		 * 生命周期函数--监听页面隐藏
		 */
		onHide: function() {},
		/**
		 * 生命周期函数--监听页面卸载
		 */
		onUnload: function() {},
		
		/**
		   * 页面相关事件处理函数--监听用户下拉动作
		*/
		onPullDownRefresh: function () {
			
		},
		/**
		 * 页面上拉触底事件的处理函数
		 */
		onReachBottom: function() {
			
		},
		/**
		 * 用户点击右上角分享
		 */
		onShareAppMessage: function() {},
		/**
		 * 分享到朋友圈
		 */
		onShareTimeline: function(e) {
			
		},
		methods: {
			//跳转详情
			toDetail: function(e) {
				var id = e.currentTarget.dataset.id;
				uni.navigateTo({
					url: '/pages/detail/detail?id=' + id
				});
			},
		}
	};
</script>
<style>
	.con-box{
		
	}
	.con-box-ziyuan {
	    width: 690rpx;
	    margin: 0 auto;
	    padding-top: 30rpx;
	}
	.con-box-ziyuan-top{
		display: flex;
		margin-bottom: 30rpx;
	}
	.con-box-ziyuan-top-name{
		font-weight: bolder;
		font-size: 34rpx;
		position: relative;
		padding-bottom: 8rpx;
		padding-left: 8rpx;
	}
	.con-box-ziyuan-top-name::after{
		background: #00b677;
		left: -5px;
		content: "";
		height: 22px;
		bottom: 3px;
		position: absolute;
		width: 4px;
	}
	.con-box-ziyuan-top-desc{
		color: #999;
		font-size: 24rpx;
		margin-left: 10rpx;
		margin-top: 12rpx;
	}
	.con-box-ziyuan-bottom{
		display: flex;
		justify-content: space-between;
		flex-wrap: wrap;
	}
	.con-box-ziyuan-list{
		width: 335rpx;
		margin-bottom: 30rpx;
	}
	.con-box-ziyuan-list-image image{
		width: 335rpx;
		height: 448rpx;
		overflow: hidden;
		border-radius: 8rpx;
	}
	.con-box-ziyuan-list-title{
		color: #333;
		margin-top: 8rpx;
		font-size: 26rpx;
		font-weight: 500;
		line-height: 21px;
		height: 42px;
		overflow: hidden;
		margin-left: 0px;
		margin-right: 0px;
		margin-bottom: 5px;
		display: -webkit-box;-webkit-box-orient: vertical;-webkit-line-clamp: 2;overflow: hidden;white-space:normal;
	}
	.con-box-ziyuan-list-bottom{
		display: flex;
		justify-content: space-between;
	}
	.con-box-ziyuan-list-bottom-num{
		color: #999;
		font-size: 24rpx;
	}
</style>
