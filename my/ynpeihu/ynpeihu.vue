<template>
	<view class="content">
		<!-- 背景图 -->
		<view class="bg">
			<image :src="swiperList[0].imageUrl" mode=""></image>
		</view>
		<!-- 服务列表 -->
		<view class="list flex justify-center" @click="gotopeople(item.serviceId,item)" :style="index==0?'margin-top: -49rpx;':''" v-for="(item,index) in classType" :key="index">
			<view class="list-box flex justify-center align-center">
				<view class="list-box-c flex justify-between align-center">
					<view class="list-box-c-l flex align-center">
						<view class="list-box-c-l-c">
							<view class="list-box-c-l-c-t">
								{{item.serviceName}}
							</view>
							<view class="list-box-c-l-c-t-b flex flex-wrap">
								<view v-for="(ite,ind) in item.tags" :key="ind">
									{{ite}} <text v-if="ind!=item.tags.length-1" class="">|</text>
								</view>
							</view>
						</view>
					</view>
					<view class="list-box-c-r">
						<span>¥</span><text>{{item.money}}</text>元/{{item.company==1?'天':'次'}}
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
				swiperList:[],
				modularId:'',
				classType:[],
				tuiName: '',
				tuiImage: '',
				invitationCode: '',
			};
		},
		onShareAppMessage(res) {
			return {
				path: '/my/ynpeihu/ynpeihu?invitation=' + this
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
				path: '/my/ynpeihu/ynpeihu?invitation=' + this.invitationCode+'&modularId='+this.modularId, //这是为了传参   onload(data){let id=data.id;} 
				title: this.tuiName,
				imageUrl: this.tuiImage
			}
		},
		onLoad(e) {
			let that = this
			this.modularId = e.modularId
			this.getBannerList()
			this.getlist();
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
		methods:{
			//选择护理员
			gotopeople(serviceId,item){
				if(uni.getStorageSync('token')){
					uni.setStorageSync('phserviceId',serviceId)
					uni.navigateTo({
						url:'/pages/index/game/gameList?name=护理员&item='+encodeURIComponent(JSON.stringify(item))
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
			//获取轮播图
			getBannerList() {
				this.$Request.get("/app/banner/selectBannerList", {
					classify: 8
				}).then(res => {
					if (res.code == 0) {
						this.swiperList = res.data
					}
				});
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
page{
	background-color: #F5F5F5;
}
.content{
	padding-bottom: 50rpx;
}
.bg{
	width: 100%;
	height: 280rpx;
	z-index: 1;
	image{
		width: 100%;
		height: 100%;
		z-index: 1;
	}
}
.list{
	width: 100%;
	height: 192rpx;
	z-index: 2;
	margin-bottom: 20rpx;
	.list-box{
		width: 686rpx;
		height: 100%;
		background-color: #ffffff;
		border-radius: 16rpx;
		z-index: 2;
		.list-box-c{
			width: 606rpx;
			height: 85rpx;
		}
		.list-box-c-l{
			image{
				width: 85rpx;
				height: 85rpx;
			}
		}
		.list-box-c-l-c-t{
			color: #333333;
			font-size: 32rpx;
			font-weight: bold;
		}
		.list-box-c-l-c-t-b{
			color: #999999;
			font-size: 26rpx;
			margin-top: 8rpx;
			text{
				margin-left: 10rpx;
				margin-right: 10rpx;
			}
		}
		.list-box-c-r{
			color: #FF2D01;
			font-size: 32rpx;
			letter-spacing:5rpx;
			span{
				font-size: 38rpx;
				font-weight: bold;
			}
			text{
				font-size: 52rpx;
				font-weight: bold;
				letter-spacing:0rpx;
			}
		}
	}
}


</style>
