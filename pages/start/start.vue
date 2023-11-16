<template>
	<view class="splash-container">
		<swiper class="splash-swiper" :indicator-dots="true">
			<swiper-item>
				<image src="../../static/start1.jpg" mode="" class="splash-image"></image>
			</swiper-item>
			<swiper-item>
				<image src="../../static/start2.jpg" mode="" class="splash-image"></image>
			</swiper-item>
			<swiper-item>
				<image src="../../static/start3.jpg" mode="" class="splash-image"></image>
			</swiper-item>
			<swiper-item>
				<image src="../../static/start4.jpg" mode="" class="splash-image"></image>
			</swiper-item>
			<swiper-item>
				<image src="../../static/start5.jpg" mode="" class="splash-image"></image>
			</swiper-item>
		</swiper>
		<view class="button">
			<text class="button-text" @click="inputDialogToggle()">网络设置</text>
		</view>
		<view class="goto">
				<button @click="toindex()">点击跳转</button>
		</view>
		<view>
			<!-- 输入框示例 -->
			<uni-popup ref="inputDialog" type="dialog">
				<uni-popup-dialog
					v-model="ip"
					ref="inputClose"
					mode="input"
					title="请输入服务器地址"
					placeholder="如:127.0.0.1:8080"
					@confirm="server"
				></uni-popup-dialog>
			</uni-popup>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			showInput: false,
			ip:''
		};
	},
	methods: {
		// 存入localStorage
		server(value){
			localStorage.setItem("baseURL",this.ip)
			console.log(value)
		},
		dialogToggle(type) {
			this.msgType = type;
			this.$refs.alertDialog.open();
		},
		dialogConfirm() {
			console.log('点击确认');
			this.messageText = `点击确认了 ${this.msgType} 窗口`;
			this.$refs.message.open();
		},
		inputDialogToggle() {
			this.$refs.inputDialog.open();
		},
		dialogClose() {
			console.log('点击关闭');
		},
		toindex(){
			uni.reLaunch({
				url:"../index/index"
			})
		}
	}
};
</script>
<style>
.splash-container {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
	display: flex;
	align-items: center;
	justify-content: center;
	background-color: #fff;
	z-index: 999;
}

.splash-swiper {
	width: 100%;
	height: 100%;
}

.splash-image {
	width: 100%;
	height: 100%;
}

.splash-button-text {
	color: #fff;
	font-size: 16px;
}

.button {
	position: absolute;
	top: 20px;
	right: 20px;
	border-radius: 10px;
}

.goto{
	position: absolute;
	bottom: 20px;
	border-radius: 10px;
	background-color: white;
	width: 80%;
}

.button-text {
	color: #fff;
}
</style>
