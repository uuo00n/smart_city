<template>
	<view>
		<view>
			<uni-section title="缴费服务" sub-title="" type="line"></uni-section>
		</view>
		<view>
			<uni-grid :column="2" :showBorder="false" :highlight="false" :square="false" @change="goSer">
				<uni-grid-item v-for="(item, index) in PaySer" :index="index" :key="index">
					<view class="grid-item-box">
						<image :src="host + item.imgUrl" mode="" class="image"></image>
						<text class="text">{{ item.liveName }}</text>
					</view>
				</uni-grid-item>
			</uni-grid>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host:'http://124.93.196.45:10001',
				PaySer:[]
			}
		},
		mounted() {
			this.getPaySer()
		},
		methods: {
			getPaySer(){
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/living/category/list',
					method: 'GET',
					header: {
						Authorization: uni.getStorageSync('token')
					},
					data: {},
					success: res => {
						console.log(res.data.data)
						this.PaySer = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goSer(item) {
				console.log(this.PaySer[item.detail.index].liveName)
				let url= '';
				if(this.PaySer[item.detail.index].liveName == '手机话费'){
					url = "../pay_phone/pay_phone"
				}
				uni.navigateTo({
					url: url
				});
			}
		}
	}
</script>

<style>
	.image {
		width: 35px;
		height: 35px;
	}
	
	.grid-item-box {
		flex: 1;
		// position: relative;
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 15px 0;
	}
</style>
