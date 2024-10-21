<template>
	<view style="padding-bottom: 50rpx;">
		<!-- 背景图 -->
		<view class="bg">
			<swiper :indicator-dots="false" :autoplay="true" style="width: 100%;height: 100%;" :interval="1000" :duration="300">
				<swiper-item v-for="(item,index) in swiperList" :key="index">
					<image :src="item.imageUrl" mode=""></image>
				</swiper-item>
			</swiper>
			
		</view>
		<!-- 服务列表 -->
		<view style="margin-top: -49upx;">
			<view class="list flex justify-center margin-bottom-sm" v-for="(item,index) in classType" :key="index" @click="goNav(item)">
				<view class="list-box flex justify-center align-center">
					<view class="list-box-c flex justify-between align-center">
						<view class="list-box-c-l flex align-center">
							<image :src="item.img?item.img:'../../static/logo.png'" mode=""></image>
							<view class="list-box-c-l-c">
								<view class="list-box-c-l-c-t">{{item.serviceName}}</view>
								<view class="list-box-c-l-c-t-b flex-wrap">
									<view v-for="(ite,ind) in item.tags" :key="ind">
										{{ite}} <text v-if="ind!=item.tags.length-1" class="">|</text>
									</view>
								</view>
							</view>
						</view>
						<!-- <view class="list-box-c-r">
							<u-icon name="arrow-right" color="#333333" size="32"></u-icon>
						</view> -->
						<view class="list-box-c-r">
							<span>¥</span><text>{{item.money}}</text>元/{{item.company==1?'天':'次'}}
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				classType: [],
				swiperList:[],
				tuiName: '',
				tuiImage: '',
				invitationCode: '',
				orderTakingUserName: '',
				orderTakingUserId: '',
			};
		},
		onShareAppMessage(res) {
			return {
				path: '/my/peizhen/peizhen?invitation=' + this
					.invitationCode+'&modularId='+this.modularId, //这是为了传参   onload(data){let id=data.id;} 
				title: this.tuiName,
				imageUrl: this.tuiImage
			}
		},
		/*
		 * uniapp微信小程序分享页面到微信朋友圈
		 */
		onShareTimeline(res) {
			return {
				path: '/my/peizhen/peizhen?invitation=' + this.invitationCode+'&modularId='+this.modularId, //这是为了传参   onload(data){let id=data.id;} 
				title: this.tuiName,
				imageUrl: this.tuiImage
			}
		},
		onLoad(e) {
			let that = this
			this.modularId = e.modularId
			this.getBannerList()
			this.getlist()
			if(e.orderTakingUserName && e.orderTakingUserId){
				this.orderTakingUserName = e.orderTakingUserName
				this.orderTakingUserId = e.orderTakingUserId
			}
			// 分享
			this.myId = uni.getStorageSync('userId')
			// 获取邀请码保存到本地
			if (e.invitation) {
				that.$queue.setData('inviterCode', e.invitation);
			}
			// #ifdef MP-WEIXIN
			if (e.scene) {
				const scene = decodeURIComponent(e.scene);
				that.$queue.setData('inviterCode', scene.split(',')[0]);
			}
			// #endif
			if (this.myId) {
				that.invitationCode = uni.getStorageSync('invitationCode')
				that.$Request.getT('/app/common/type/276').then(res => { //分享标题 276
					if (res.code == 0) {
						that.tuiName = res.data.value
					}
				})
				that.$Request.getT('/app/common/type/277').then(res => { //分享图 277
					if (res.code == 0) {
						that.tuiImage = res.data.value
					}
				})
			}
		},
		methods: {
			//获取轮播图
			getBannerList() {
				this.$Request.get("/app/banner/selectBannerList", {
					classify: 7
				}).then(res => {
					if (res.code == 0) {
						this.swiperList = res.data
					}
				});
			},
			goNav(e) {
				let that = this
				if(uni.getStorageSync('token')){
					uni.navigateTo({
						url: '/pages/index/game/orderDet?serviceId='+e.serviceId+'&serviceType='+e.serviceType+'&orderTakingUserName='+that.orderTakingUserName+'&orderTakingUserId='+that.orderTakingUserId
					})
				}else{
					uni.showModal({
						title: '提示',
						content: '您还未登录,请先登录',
						success: function(res) {
							if (res.confirm) {
								console.log('用户点击确定');
								uni.navigateTo({
									url: '/pages/public/login'
								})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					})
				}
				
			},
			getlist() {
				let data ={
					modularId:this.modularId
				}
				this.$Request.get('/app/hospitalEmploy/getHospitalEmployList',data).then(res => {
					if (res.code == 0) {
						this.classType = res.data.records
						this.classType.map(item=>{
							if(item.tags){
								item.tags = item.tags.split(',')
							}else{
								item.tag = []
							}
						})
					}

				});
			}
		}
	}
</script>

<style lang="scss">
	page {
		background-color: #F5F5F5;
	}

	.bg {
		width: 100%;
		height: 280rpx;
		z-index: 1;

		image {
			width: 100%;
			height: 100%;
			z-index: 1;
		}
	}

	.list {
		width: 100%;
		height: 192rpx;
		z-index: 2;
		margin-bottom: 20rpx;

		.list-box {
			width: 686rpx;
			height: 100%;
			background-color: #ffffff;
			border-radius: 16rpx;
			z-index: 2;

			.list-box-c {
				width: 606rpx;
				height: 85rpx;
			}

			.list-box-c-l {
				image {
					width: 85rpx;
					height: 85rpx;
				}
			}

			.list-box-c-l-c {
				margin-left: 26rpx;
			}

			.list-box-c-l-c-t {
				color: #333333;
				font-size: 32rpx;
				font-weight: bold;
			}

			.list-box-c-l-c-t-b {
				color: #999999;
				font-size: 26rpx;
				margin-top: 8rpx;
				display: flex;
				align-items: center;

				text {
					margin-left: 10rpx;
					margin-right: 10rpx;
				}
			}
			.list-box-c-r{
				width: 220upx;
				text-align: right;
				color: #FF2D01;
				font-size: 32rpx;
				letter-spacing:5rpx;
				span{
					font-size: 38rpx;
					font-weight: bold;
				}
				text{
					font-size: 48rpx;
					font-weight: bold;
					letter-spacing:0rpx;
				}
			}
		}
	}
</style>
