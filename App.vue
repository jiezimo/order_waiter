<script>
	export default {
		// 生命周期函数
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		},
		onError:function(err){
			console.log('出现异常了', err)
		}
	}
</script>

<style>
	/*每个页面公共css */
</style>
