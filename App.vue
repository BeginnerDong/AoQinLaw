
<script>
	import token from "./common/token.js"
	export default {
		onLaunch: function() {	
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	@import "/assets/style/style.css";
	.ql-editor img{
		max-width: 100%;
		min-width: 50%;
		display: block;
		padding: 10px 0;
	}
	@import "/assets/style/quill.css";
	
</style>
