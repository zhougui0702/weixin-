<template>
    <view class="product-con">
        <view id="past0">
            <view class="wrapper">
                <view class="wrapper-top">
                    <view class="wrapper-top-left">
                        <image src="/static/logo.png"></image>
                    </view>
                    <view class="wrapper-top-center">
                        <view class="wrapper-top-center-name">懒人源码</view>
                        <view class="wrapper-top-center-time">
							<text v-for="(item, index) in storeInfo['label']" @click="to_search" :data-text="item">#{{item}}</text>
						</view>
                    </view>
                    <view class="wrapper-top-right">
                        <view class="wrapper-top-right-number">{{ storeInfo['click'] }}人查看</view>
                        <view class="wrapper-top-right-number">{{ storeInfo['download_number'] }}人想要</view>
                    </view>
                </view>
            </view>
        </view>

        <view class="product-intro">
			<view class="product-intro-name">
				<view
					@longpress="copying_title"
					:data-txt="storeInfo.store_name"
				>
					{{ storeInfo.store_name }}
				</view>
			</view>
            <view class="conter wxParse product-intro-con">
                <view
                    @longpress="copying"
                    :data-txt="storeInfo.description"
                    :style="platform == 'devtools' ? 'margin-top: -0rpx;white-space: pre-line;' : 'margin-top: -0rpx;white-space: pre-line;'"
                >
                    {{ storeInfo.description }}
                </view>
            </view>
			
            <view class="product-intro-swiper">
                <image :data-src="item" @tap="previewImage" :src="item" mode="widthFix" v-for="(item, index) in storeInfo.slider_image" :key="index"></image>
            </view>
			
        </view>
		
        <view class="product-intro product-intro-xuzhi">
            <view class="title">下载须知</view>
            <view class="conter xuzhi">
                <view class="xuzhi-box">
					<rich-text :nodes="storeInfo.remark"></rich-text>
                </view>
            </view>
        </view>
		
        <view style="height: 150rpx"></view>
		
		<!-- #ifdef H5 -->
		<view class="footer acea-row row-between-wrapper">
			<block v-if="storeInfo['type']==3">
				<view @tap="goBuy" class="acea-row-button">联系客服</view>
			</block>
			<block v-else>
				
				<view class="con-box-bottom">
					<view class="con-box-bottom-left">
						<view v-if="storeInfo['adv_status']-0>0" @tap="goBuy" class="acea-row-button">观看视频后直接下载</view>
						<view v-else @tap="goBuy" class="acea-row-button">已看广告，点我下载</view>
					</view>
				</view>
				
			</block>
		</view>
		<!-- #endif -->
		
    </view>
</template>

<script>

const app = getApp();

export default {
    data() {
        return {
            id: 0,
            //商品id
            storeInfo: {
                
            },
        };
    }
    /**
     * 生命周期函数--监听页面加载
     */,
    onLoad: function (options) {
		var that = this;
		var id = options['id'];
		that.id = id;
        uni.showLoading({
          title: '加载中',
        })
        //获取接口数据---接口不同可联系微信【dongli3023】
        uni.request({
        	url: 'https://ziyuan.03023.net//api/product/detail/'+id,
        	data: {
        		
        	},
        	method: 'GET',
        	header: {
        		'Content-Type': 'application/x-www-form-urlencoded',
        	},
        	success: res => {
        	    uni.hideLoading();
        		console.log(res);
        	    var storeInfo = res.data.data.storeInfo;
        	    that.storeInfo = storeInfo;
        	},
        	fail: function (res) {
        		console.log(res);
        	}
        });
    },
	
	onShow: function (options) {
		
	},
    /**
     * 用户点击右上角分享
     */
    onShareAppMessage: function () {
        
    },
    
    /**
     * 分享到朋友圈
     */
    onShareTimeline: function (e) {
        
    },
    methods: {
		/**
         * 免费下载
         */
        goBuy: function (e) {
            var that = this;
			// #ifdef H5
			if(true){
				var appid = "wxec4daa353970610d";
				var path = "pages/look_adv/look_adv";
				var deviceId = 'uniapp';
				var articleId = that.id;
				
				var query = encodeURIComponent("deviceId="+deviceId+"&articleId="+articleId);
				window.location.href="weixin://dl/business/?appid="+appid+"&path="+path+"&query="+query;
			}
			// #endif
        },
    }
};
</script>
<style>
	.product-con .footer {
	    padding: 0 20rpx 0 30rpx;
	    position: fixed;
	    bottom: 0;
	    width: 100%;
	    box-sizing: border-box;
	    height: 150rpx;
	    background-color: #fff;
	    z-index: 8999;
	}
	.acea-row-button-box{
		display: flex;
		justify-content: space-between;
	}
	.acea-row-button {
	    background: #67c23a;
	    height: 80rpx;
	    line-height: 80rpx;
	    border-radius: 50rpx;
	    font-size: 30rpx;
	    width: 654rpx;
	    margin: 20rpx auto;
	    text-align: center;
	    color: #fff;
	}
	.acea-row-button-box .acea-row-button{
		width: 400rpx !important;
		margin-left: 0;
	}
	.acea-row-button-show{
		background: #409eff;
		height: 80rpx;
		line-height: 80rpx;
		border-radius: 50rpx;
		font-size: 30rpx;
		width: 200rpx;
		margin-top: 20rpx;
		text-align: center;
		margin-right: 8rpx;
		color: #fff;
	}
	.product-intro {
	    width: 690rpx;
	    margin: 30rpx auto;
	    color: #666;
	}
	.product-intro-con {
	    line-height: 50rpx;
	    font-size: 28rpx;
	    margin-bottom: 15rpx;
	}
	.product-intro-con text {
	    text-emphasis: none;
	}
	.product-intro-xuzhi {
	    border-top: 1rpx solid #ebeef5;
	    padding-top: 30rpx;
	}
	.product-intro .title {
	    font-size: 30rpx;
	}
	.xuzhi-box {
	    margin-top: 20rpx;
	}
	.product-intro .xuzhi-list {
	    font-size: 28rpx;
	    line-height: 50rpx;
	    margin-bottom: 10rpx;
	}
	.product-intro-swiper {
	    margin-top: 15rpx;
	}
	.product-intro-swiper image {
	    width: 100%;
	    border-radius: 10rpx;
	}
	.conx-box-mask {
	    position: fixed;
	    left: 0;
	    top: 0;
	    width: 100%;
	    height: 100%;
	    background: rgba(0, 0, 0, 0.6);
	    z-index: 99999999;
	}
	.conx-box-form {
	    position: fixed;
	    width: 654rpx;
	    height: 520rpx;
	    left: 50%;
	    margin-left: -328rpx;
	    top: 50%;
	    margin-top: -300rpx;
	    background: #f9f9f9;
	    z-index: 999999999;
	    border-radius: 10rpx;
	}
	.conx-box-form-box-inner {
	    width: 600rpx;
	    margin: 0 auto;
	}
	.conx-box-form-box-top {
	    height: 100rpx;
	    line-height: 100rpx;
	    text-align: center;
	    font-size: 32rpx;
	    border-bottom: 1rpx solid #eee;
	}
	.conx-box-form-box-bottom {
	    margin-top: 50rpx;
	    background: #e8e8e8;
	    border-radius: 28rpx;
	    width: 100%;
	}
	.conx-box-form-box-bottom textarea {
	    width: 95%;
	    height: 150rpx;
	    padding: 20rpx 15rpx;
	    color: #666;
	    line-height: 50rpx;
		font-size:28rpx;
		line-break: anywhere;
	}
	.conx-box-form-box-bottom-list {
	    margin-bottom: 30rpx;
	    border-bottom: 1rpx solid #eee;
	    padding-bottom: 30rpx;
	}
	.conx-box-form-box-bottom-list input {
	    font-size: 28rpx;
	}
	.conx-box-form-box-button {
	    width: 540rpx;
	    height: 80rpx;
	    line-height: 80rpx;
	    color: #fff;
	    text-align: center;
	    background: #67c23a;
	    font-size: 30rpx;
	    margin: 50rpx auto 0;
	    border-radius: 40rpx;
	}
	
	.wrapper-top {
	    display: flex;
	    justify-content: space-between;
	    margin: 0rpx auto;
	    width: 690rpx;
	    padding-top: 30rpx;
	    padding-bottom: 30rpx;
	    border-bottom: 1rpx solid #ebeef5;
	}
	.wrapper-top-left image {
	    width: 96rpx;
	    height: 96rpx;
	    border-radius: 50%;
	}
	.wrapper-top-center {
	    flex: 1;
	    margin-left: 30rpx;
	}
	.wrapper-top-center-name {
	    font-size: 30rpx;
	}
	.wrapper-top-center-time {
	    font-size: 26rpx;
	    color: #666;
	    margin-top: 20rpx;
	}
	
	.wrapper-top-right {
	    margin-left: 30rpx;
	}
	.wrapper-top-right-number {
	    font-size: 24rpx;
	    color: #999;
	    display: flex;
	    margin-bottom: 22rpx;
	}
	.wrapper-top-right-number-left {
	    width: 40rpx;
	    height: 40rpx;
	}
	.wrapper-top-right-number-left image {
	    width: 40rpx;
	    height: 40rpx;
	}
	.wrapper-top-right-number-right {
	    height: 40rpx;
	    line-height: 40rpx;
	    margin-left: 5rpx;
	}
	
</style>
