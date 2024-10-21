<template>
	<view style="padding-bottom: 30rpx;">
		<view class="cont flex justify-center">
			<view class="cont-box">
				<u-parse :html="content"></u-parse>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				content:'',
			};
		},
		onLoad(option) {
			//313  陪诊
			//314  陪护
			if(option.type==1){
				this.getxy()
			}else{
				this.getxyph()
			}
		},
		methods:{
			//陪诊
			getxy(){
				this.$Request.get('/app/common/type/313').then(res => {
					if (res.code == 0) {
						this.content = res.data.value
					}
				});
			},
			//陪护
			getxyph(){
				this.$Request.get('/app/common/type/314').then(res => {
					if (res.code == 0) {
						this.content = res.data.value
					}
				});
			},
		}
	}
</script>

<style lang="scss">
.cont{
	width: 100%;
	.cont-box{
		width: 686rpx;
		margin-top: 30rpx;
	}
}
</style>
