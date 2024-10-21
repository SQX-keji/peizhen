<script>
	export default {
		globalData: {
			tabIndex: ''
		},
		onLaunch: function() {

		},
		onShow: function() {
			// #ifdef MP-WEIXIN
			//小程序是否上线
			this.$queue.setData('XCXIsSelect', '是');
			// #endif

			//企业微信客服链接
			this.$Request.get('/app/common/type/274').then(res => {
				if (res.code == 0) {
					this.$queue.setData('kefu', res.data.value);
				}
			});
			//企业微信客服appid
			this.$Request.get('/app/common/type/275').then(res => {
				if (res.code == 0) {
					this.$queue.setData('kefuAppId', res.data.value);
				}
			});
			this.$queue.setData('dailishang', '是');
			//客服类型  328
			this.$Request.get('/app/common/type/328').then(res => {
				if (res.code == 0) {
					this.$queue.setData('SelKeFu', res.data.value);
				}
			});
			//客服电话  329
			this.$Request.get('/app/common/type/329').then(res => {
				if (res.code == 0) {
					this.$queue.setData('kefuPhone', res.data.value);
				}
			});

			this.$queue.setData('czSel', '是');

			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style lang="scss">
	/*每个页面公共css */
	@import "uview-ui/index.scss";
	@import 'components/colorui/main.css';
	@import 'components/colorui/icon.css';

	page {
		background-color: #FFFFFF;
		color: #343546;
	}

	.bg {
		background-color: #F7F7F7;
	}
</style>