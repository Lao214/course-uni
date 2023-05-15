<template>
	<view class="">
		<view class="header">
			<h1 style="margin-right: 30rpx;">笔记</h1>
			<h1 style="color: #2c30306e;" @click="gotoToDoList">待办事项</h1>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			gotoToDoList() {
				uni.navigateTo({
					url: '/pages/index/index'
				})
			}
		}
	}
</script>

<style>
.header {
	display: flex;
	padding-left: 15px;
	padding-top: 15px;
	font-family: 'Courier New', Courier, monospace;
}
page {
	background-color: #efeef1;
}
</style>
