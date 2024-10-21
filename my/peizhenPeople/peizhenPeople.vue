<template>
	<view style="padding-bottom: 100rpx;" :style="people.length==0?'padding-top: 200rpx;':''">
		<!-- 搜索 -->
		<!-- #ifdef H5 -->
		<view class="search flex justify-center align-center" style="top: 80rpx;">
			<!-- #endif -->
			<!-- #ifndef H5 -->
			<view class="search flex justify-center align-center">
				<!-- #endif -->
				<view class="search-box">
					<u-search placeholder="搜索陪诊师" @clear="searchPeople" @search="searchPeople" @custom="searchPeople"
						v-model="keyword"></u-search>
				</view>

			</view>
			<!-- 筛选 -->
			<!-- #ifdef H5 -->
			<view class="sx" style="top: 156rpx;">
				<!-- #endif -->
				<!-- #ifndef H5 -->
				<view class="sx">
					<!-- #endif -->

					<u-dropdown height="80rpx" ref="uDropdown">
						<u-dropdown-item @change="sexFunction()" v-model="sex" title="性别"
							:options="options1"></u-dropdown-item>
						<u-dropdown-item @change="sexFunction()" v-model="age" title="年龄"
							:options="options3"></u-dropdown-item>
						<u-dropdown-item @change="sexFunction()" v-model="orderCount" title="接单量" :options="options2">
						</u-dropdown-item>
						<u-dropdown-item @change="sexFunction()" v-model="finalScore" title="师傅评分" :options="options4">
						</u-dropdown-item>
					</u-dropdown>
				</view>

				<!-- 护工列表 -->
				<view class="list flex justify-center flex-wrap" style="margin-top: 180rpx;" v-if="people.length>0">
					<view class="list-box">
						<view class="list-box-item flex justify-between flex-wrap" @click="goInfo(item.userId)"
							v-for="(item,index) in people" :key="index">
							<view class="flex align-center" style="height: 100%;">
								<image
									:src="item.userCertification && item.userCertification.avatar?item.userCertification.avatar:'../../static/logo.png'"
									mode=""></image>
								<view class="list-box-item-info">
									<view class="flex align-center">
										{{item.userCertification?item.userCertification.name:''}}
										<image v-if="item.iconImg"
											style="width: 30rpx;height: 30rpx;margin-left: 20rpx;border-radius: 0;"
											:src="item.iconImg" mode="widthFix"></image>
									</view>
									<view class="">
										{{item.userCertification.sex==2?'女':'男'}} | <text
											style="margin: 0 10rpx 0 10rpx;"
											v-if="item.userCertification && item.userCertification.age">{{item.userCertification.age}}岁</text>
										<text style="margin: 0 10rpx 0 10rpx;" v-else>暂无</text> | <text
											style="margin: 0 10rpx 0 10rpx;"
											v-if="item.userCertification && item.userCertification.province">{{item.userCertification.province}}
											{{item.userCertification.city}}</text> <text
											style="margin: 0 10rpx 0 10rpx;" v-else>暂无</text>
									</view>
									<view class="flex align-center" style="line-height: 30rpx;">
										<u-rate :count="count" v-model="value" active-color="#FFAA01"
											inactive-color="#FFAA01"
											style="margin-left: -5rpx;"></u-rate>{{item.finalScore}}
										<text style="margin-left: 20rpx;"
											v-if="item.orderCount+''">接单量:{{item.orderCount}}单</text>
										<text style="margin-left: 20rpx;" v-else>接单量:0单</text>
									</view>

								</view>
							</view>
							<view class="list-box-item-status flex justify-center align-center"
								:style="item.workStatus!=0?'background:red;':''">
								{{item.workStatus==0?'空闲':'忙碌'}}
							</view>
							<view class="list-box-item-fw" v-if="item.userCertification">
								服务项目：{{item.userCertification.serviceName}}
							</view>
							<view class="list-box-item-xz flex justify-end align-center">
								<view class="list-box-item-xz-sub" @click.stop="goBack(item)">
									选择TA
								</view>
							</view>
						</view>
					</view>

					<u-loadmore v-if="people.length>5" :status="status" :icon-type="iconType" :load-text="loadText" />
				</view>
				<empty v-if="people.length==0" />
			</view>
</template>

<script>
	import empty from '@/components/empty.vue'
	export default {
		components: {
			empty
		},
		data() {
			return {
				keyword: '',
				status: 'loadmore',
				iconType: 'flower',
				loadText: {
					loadmore: '轻轻上拉',
					loading: '努力加载中',
					nomore: '实在没有了'
				},
				value: 1,
				count: 1,
				sex: '不限',
				orderCount: '', //接单量
				finalScore: '', //师傅评分
				age: '不限',
				options1: [{
						label: '不限',
						value: '不限',
					},
					{
						label: '男',
						value: '男',
					},
					{
						label: '女',
						value: '女',
					}
				],
				options2: [{
						label: '不限',
						value: '',
					},
					{
						label: '从低到高',
						value: 2,
					},
					{
						label: '从高到低',
						value: 1,
					},
				], //接单量
				options4: [{
						label: '不限',
						value: '',
					},
					{
						label: '从低到高',
						value: '0',
					},
					{
						label: '从高到低',
						value: '1',
					},
				], //师傅评分
				options3: [{
						label: '年龄',
						value: 1,
					},
					{
						label: '加冰',
						value: 2,
					},
				], //年龄
				people: [],
				page: 1,
				limit: 10,
				total: '',
				sexs: '', // 性别 1男 2女
				ageMin: '', //年龄最小值
				ageMax: '', //年龄最大值

			}
		},
		onShow(option) {
			this.getnl();
			this.getpeopleList();
		},
		methods: {
			//陪珍师详情
			goInfo(userId) {
				if (!uni.getStorageSync('token')) {
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
				} else {
					uni.navigateTo({
						url: '/my/introduction/introduction?userId=' + userId + '&back=true'
					})
				}
			},
			// 搜索陪珍师
			searchPeople() {
				this.getpeopleList();
			},
			// 携带参数跳转
			goBack(item) {
				if (!uni.getStorageSync('token')) {
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
				} else {
					if (item.workStatus == 0) { //不忙碌
						let objInfo = {
							orderTakingUserName: item.userCertification.name,
							orderTakingUserId: item.userCertification.userId
						}
						uni.$emit('peizhenPeople', objInfo)
						uni.navigateBack()
					} else {
						uni.showToast({
							title: '请选择空闲状态的护理员',
							icon: 'none'
						})
					}
				}
			},
			//护工列表
			getpeopleList() {
				let data = {
					page: this.page,
					limit: this.limit,
					sex: this.sexs, //性别
					ageMin: this.ageMin, //最小年龄
					ageMax: this.ageMax, //最大年龄
					orderCount: this.orderCount, //师傅接单量
					finalScore: this.finalScore, //师傅评分
					city: uni.getStorageSync('city'), //城市
					authentication: 1, //陪诊员，
					realName: this.keyword

				}
				this.$Request.get("/app/user/getNursingListV5", data).then(res => {
					uni.stopPullDownRefresh()
					if (res.code == 0) {
						this.total = res.data.pages
						if (this.page == 1) {
							this.people = res.data.records
						} else {
							this.people = [...this.people, ...res.data.records]
						}

					}
				})
			},
			//年龄回调
			sexFunction(e) {
				if (this.sex == '男') {
					this.sexs = 1
				} else if (this.sex == '女') {
					this.sexs = 2
				} else {
					this.sexs = ''
				}

				//年龄
				let x = this.age.indexOf('-')
				let z = this.age.indexOf('岁')
				if (x != -1) {
					this.ageMin = this.age.substring(0, x)
					this.ageMax = this.age.substring(x + 1, z)
				} else {
					this.ageMin = this.age.substring(0, z)
					this.ageMax = ''
				}
				this.getpeopleList();
			},
			getnl() {
				let data = {
					type: '年龄'
				}

				this.$Request.get("/app/dict/selectDictList", data).then(res => {
					if (res.code == 0) {
						let arr = JSON.parse(JSON.stringify(res.data).replace(/code/g, 'label'))
						this.options3 = arr
					}
				})
			},
		},
		onReachBottom: function() {
			if (this.page < this.total) {
				this.page += 1
				this.status = 'loading'
				this.getpeopleList()
			} else {
				this.status = 'nomore'
			}
		},
		onPullDownRefresh: function() {
			this.page = 1
			this.getpeopleList()
		},
	}
</script>

<style lang="scss">
	page {
		background-color: #F5F5F5;
	}

	.search {
		width: 100%;
		height: 80rpx;
		background-color: #ffffff;
		position: fixed;
		top: 0;
		z-index: 999;

		.search-box {
			width: 686rpx;
			height: 100%;
		}
	}

	.sx {
		width: 100%;
		height: 80rpx;
		background-color: #ffffff;
		position: fixed;
		top: 76rpx;
		z-index: 999;
	}

	.list {
		width: 100%;
		height: auto;
		z-index: 1;

		.list-box {
			width: 686rpx;
			height: auto;

			.list-box-item {
				width: 100%;
				// height: 200rpx;
				background-color: #ffffff;
				border-radius: 16rpx;
				margin-bottom: 20rpx;
				padding: 20rpx 0;

				image {
					width: 160rpx;
					height: 160rpx;
					border-radius: 16rpx;
					margin-left: 20rpx;
					z-index: 1;
				}

				.list-box-item-info {
					margin-left: 20rpx;

					view:nth-of-type(1) {
						color: #1E1F31;
						font-size: 32rpx;
						font-weight: bold;
					}

					view:nth-of-type(2) {
						color: #333333;
						font-size: 26rpx;
						font-weight: 500;
						margin-top: 20rpx;
					}

					view:nth-of-type(3) {
						color: #333333;
						font-size: 24rpx;
						font-weight: 500;
						margin-top: 20rpx;
					}
				}

				.list-box-item-status {
					width: 85rpx;
					height: 36rpx;
					background: #468EF8;
					border-radius: 18rpx 0px 0px 18rpx;
					color: #FFFFFF;
					font-size: 24rpx;
					font-weight: 500;
					margin-top: 36rpx;
				}

				.list-box-item-fw {
					padding-left: 200rpx;
					margin-top: 20rpx;
					padding-right: 20rpx;
				}

				.list-box-item-xz {
					width: 100%;
					padding-right: 20rpx;
					margin-top: 20rpx;

					.list-box-item-xz-sub {
						padding: 10rpx 30rpx;
						background-color: #468EF8;
						color: #ffffff;
						border-radius: 40rpx;
					}
				}
			}
		}
	}
</style>