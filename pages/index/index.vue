<template>
	<view class="content">
		<button class="content-btn">上传文档文件</button>
		<view class="content-bottom">
			<view>使用方法：</view>
			<view>1、从手机本地文件夹上传文档文件；</view>
			<view>2、苹果手机可先将文档文件发送给传输助手，点击上传，从聊天会话中选择传输助手，然后选择需要解析的语音；</view>
			<view>3、目前支持的文档格式有：PDF、word、ppt、txt。</view>
			<button class="doc-demo-btn" @click="checkDoc">
				<view class="doc-demo-btn-left">
					<image class="doc-icon" src="../../static/doc.png"></image>
					示例文档文件.pdf
				</view>
				<view class="doc-demo-btn-right">&gt;</view>
			</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello',
			}
		},
		onLoad() {

		},
		methods: {
			checkDoc() { // 查看文档文件
				uni.navigateTo({ url: '/pages/chat/chat' });
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 100%;
	}
	.content-btn {
		width: 70%;
		height: 96rpx;
		line-height: 96rpx;
		font-size: 28rpx;
		color: #ffffff;
		background-color: #23a3ff;
		border-radius: 60rpx;
	}
	.content-bottom {
		position: absolute;
		left: 0;
		right: 0;
		bottom: 0;
		padding: 40rpx;
		font-size: 24rpx;
		color: #666;
	}
	.doc-demo-btn {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-top: 20rpx;
		background-color: #e5f3fe;
		border-color: #e5f3fe;
	}
	.doc-demo-btn-left {
		display: flex;
		align-items: center;
		font-size: 24rpx;
	}
	.doc-icon {
		padding: 4rpx;
		margin-right: 12rpx;
		width: 32rpx;
		height: 32rpx;
		border-radius: 50%;
		background-color: #ffffff;
	}
</style>
