<template>
	<view>
		<!-- 定位城市 -->
		<view class="city flex justify-center">
			<view class="city-box flex justify-between align-center">
				<view class="" @click="cityClick(city)">
					{{city}}
				</view>
				<view class="" @click="getMap">
					<u-icon name="map" color="#000000" size="26" style="margin-right: 10rpx;"></u-icon>重新定位
				</view>
			</view>
		</view>
		<!-- 城市列表选择 -->
		<view class="cityList flex justify-center">
			<view class="cityList-box">
				<scroll-view scroll-y="true" style="width: 100%;height: 100%;" @scroll="scroll">
					<u-index-list :scrollTop="scrollTop" offset-top="200">
						<view v-for="(item, index) in indexList" :key="index">
							<u-index-anchor :index="item.letter" />
							<view class="list-cell" v-for="(ite,ind) in item.city" :key="ind" @click="cityClick(ite)">
								{{ite}}
							</view>
						</view>
					</u-index-list>
				</scroll-view>
				
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				latitude: '',
				longitude: '',
				city: '',
				scrollTop: 0,
				indexList: ["A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R", "S",
					"T", "U", "V", "W", "X", "Y", "Z"
				],
			};
		},
		onLoad() {
			//获取定位
			this.getMap()
			//获取城市列表
			this.getCityList()
		},
		methods: {
			//修改当前城市
			cityClick(city) {
				uni.showToast({
					icon: 'none',
					title: '修改成功',
					mask: true
				})
				uni.setStorageSync('city', city)
				setTimeout(function() {
					let pages = getCurrentPages(); // 当前页面
					let beforePage = pages[pages.length - 2]; // 上一页
					uni.navigateBack({
						success: function() {
							beforePage.onShow(); // 执行上一页的onShow方法
						}
					});
				}, 1000)
			},
			//获取城市数据
			getCityList(){
				this.$Request.get('/app/hospital/getCityList').then(res => {
					if (res.code == 0) {
						this.indexList = res.data
					}
				});
			},
			//同步滑动的高度
			scroll(e){
				this.scrollTop = e.detail.scrollTop;
			},
			//获取定位
			getMap() {
				let that = this
				uni.getLocation({
					type: 'gcj02', //wgs84  gcj02
					success: function(res) {
						// console.log(res, '地理位置')
						that.latitude = res.latitude
						that.longitude = res.longitude
						uni.setStorageSync('latitude', res.latitude)
						uni.setStorageSync('longitude', res.longitude)

						// #ifdef MP-WEIXIN
						uni.request({
							url: 'https://apis.map.qq.com/ws/geocoder/v1/?location=' + that.latitude +
								',' + that.longitude + '&key=O2PBZ-6J3CX-GWK44-TXGQL-QKC2T-2UBP6',
							success(re) {
								console.log(re)
								if (re.statusCode === 200) {
									let citydata = re.data.result.address_component.city
									// console.log("获取城市名称成功", citydata)
									that.city = citydata ? citydata : '未知'
									uni.setStorageSync('city', citydata)
								} else {
									console.log("获取信息失败，请重试！")
								}
							}
						});
						// #endif

						// #ifdef APP-PLUS
						console.log(res,'*****************')
						if (res.address) {
							that.city = res.address.city
							uni.setStorageSync('city', res.address.city)
						} else {
							that.selectCity(that.longitude, that.latitude);
						}
						// #endif

						// #ifdef H5
						that.selectCity(that.longitude, that.latitude);
						// #endif


					},
					fail: function(err) {
						console.log(err,'获取地址失败')
					}
				})
			},
			selectCity(longitude, latitude) {
				this.$Request.get('/app/Login/selectCity?lat=' + latitude + '&lng=' + longitude).then(res => {
					if (res.code == 0) {
						console.log(res,'+++++++++++++++++++')
						this.city = res.data.result ? res.data.result.ad_info.city : '未知'
						uni.setStorageSync('city', this.city)
					}
				});
			},
		},
	}
</script>

<style lang="scss">
	page {
		background-color: #F7F7F7;
		overflow-y: hidden;
	}

	.city {
		width: 100%;
		height: 80rpx;
		background-color: #ffffff;
		position: fixed;
		/* #ifdef H5 */
		top: 108rpx;
		/* #endif */
		/* #ifndef H5 */
		top: 20rpx;

		/* #endif */
		.city-box {
			width: 710rpx;
			height: 100%;
			font-size: 26rpx;
		}
	}

	.list-cell {
		display: flex;
		box-sizing: border-box;
		width: 100%;
		padding: 10px 24rpx;
		overflow: hidden;
		color: #323233;
		font-size: 14px;
		line-height: 24px;
		background-color: #fff;
	}

	.cityList {
		position: fixed;
		/* #ifdef H5 */
		top: 208rpx;
		/* #endif */
		/* #ifndef H5 */
		top: 100rpx;
		/* #endif */
		width: 100%;
		height: calc(100vh - 228rpx);
		background-color: #ffffff;
		.cityList-box {
			width: 100%;
			height: 100%;
		}
	}
</style>
