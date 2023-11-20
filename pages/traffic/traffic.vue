<template>
	<view>
		<view>
			<uni-section title="交管服务" sub-title="" type="line"></uni-section>
		</view>
		<view>
			<uni-grid :column="2" :showBorder="true" :highlight="true" :square="false" @change="tapTypeItem">
				<uni-grid-item v-for="(item, index) in type" :index="index" :key="index">
					<view class="grid-item-box">
						<text>{{item}}</text>
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
				type:['违章查询','违章记录']
			}
		},
		methods: {
			tapTypeItem(e){
				if(e.detail.index == 0){
					uni.navigateTo({
						url: '../traffic_search/traffic_search',
						success: res => {},
						fail: () => {},
						complete: () => {}
					});
				}else{
					uni.navigateTo({
						url: '../traffic_record/traffic_record',
						success: res => {},
						fail: () => {},
						complete: () => {}
					});
				}
			}
		}
	}
</script>

<style>
.grid-item-box {
		flex: 1;
		// position: relative;
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		align-items: center;
		justify-content: center;
		padding: 15px 0;
	}
</style>
