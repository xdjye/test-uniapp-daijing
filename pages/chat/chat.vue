<template>
	<view class="chat">
		<view class="chat-top">
			<view class="chat-top-left">
				<image class="doc-icon" src="../../static/doc.png"></image>
				示例文档文件.pdf
			</view>
			<view class="chat-top-right">
				<view class="chat-top-right-btn">
					<image class="oper-icon" src="../../static/export.png"></image>
					<button class="chat-top-operBtn" plain type="default" size="mini">导出</button>
				</view>
				<view class="chat-top-right-btn">
					<image class="oper-icon" src="../../static/copy.png"></image>
					<button class="chat-top-operBtn" plain type="default" size="mini">复制</button>
				</view>
			</view>
		</view>
		<view class="chat-content" id="mainel">
			<view class="chat-content-tips">
				<view class="chat-content-tips-title">{{ tipsInfo.title }}</view>
				<view class="chat-content-tips-list">
					<view class="chat-content-tips-list-li" v-for="(item, index) in tipsInfo.contents" :key="index">{{ item }}</view>
				</view>
			</view>
			<view class="chat-content-text">
				<view class="chat-text-li" v-for="(item, index) in chatList" :class="item.type ? 'chat-text-P' : 'chat-text-AI'">
					<view class="chat-title">{{ item.type ? "提问者" : "嚼字机器人" }}</view>
					<view class="chat-textValue">{{ item.texts }}</view>
				</view>
			</view>
		</view>
		<view class="chat-bottom" :class="{'key-board-height': keyBoardHeightFlag}">
			<input class="bottom-input" placeholder="问些文档相关的问题吧" v-model="inputValue" @confirm="communicateAI"/>
			<button class="submit-btn" size="mini" @click="communicateAI">发送</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				keyBoardHeightFlag: false,
				inputValue: null,
				tipsInfo: {
					title: "你可以尝试这样问：",
					contents: [
						"简单描述一下这份文件讲了些什么内容？",
						"这些项目中哪些是最重要的，需要特别关注？",
						"这份文件可以用于哪些方面的青少年健康评估？",
					]
				},
				chatList: []
			}
		},
		onLoad() {
			uni.onKeyboardHeightChange(this.onKeyboardHeightChange);
		},
		onUnload() {
		  uni.offKeyboardHeightChange(this.onKeyboardHeightChange);
		},
		methods: {
			onKeyboardHeightChange(e) {
				const { height, duration } = e;
				// 键盘弹起
				if (height > 0) {
				    this.keyBoardHeightFlag = true;
				}
				// 键盘收回
				else {
					this.keyBoardHeightFlag = false;
				}
			},
			mainScroll() {
				const mainEl = window.document?.getElementById("mainel");
				console.log(mainEl);
				mainEl?.scrollTo({ top: mainEl.scrollHeight, Behavior: 'smooth' });
			},
			// 与机器人交互
			communicateAI() {
				if(!this.inputValue) {
					uni.showToast({
						title: '请输入您的问题！',
						duration: 2000,
						icon: "none"
					});
					return
				}
				this.chatList.push({ type: 1, texts: this.inputValue })
				this.inputValue = null
				uni.request({
					url: 'https://api.vvhan.com/api/joke', //仅为示例，并非真实接口地址。
					success: (res) => {
						this.chatList.push({ type: 0, texts: res.data })
					}
				});
				setTimeout(() => { this.mainScroll() }, 300)
				// this.mainScroll();
			}
		}
	}
</script>

<style>
	.chat {
		display: flex;
		flex-direction: column;
		width: 100%;
		height: 100%;
	}
	.chat-top {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 28rpx 20rpx;
	}
	.chat-top-left {
		flex: 1;
		display: flex;
		align-items: center;
		padding:  20rpx 20rpx;
		font-size: 24rpx;
		background-color: #e5f3fe;
		border-radius: 16rpx;
	}
	.doc-icon {
		padding: 4rpx;
		margin-right: 12rpx;
		width: 32rpx;
		height: 32rpx;
		border-radius: 50%;
		background-color: #ffffff;
	}
	.chat-top-right {
		display: flex;
		align-items: center;
		margin-left: 40rpx;
	}
	.chat-top-right-btn {
		display: flex;
		flex-direction: column;
		align-items: center;
	}
	.chat-top-right-btn:last-child {
		margin-left: 40rpx;
	}
	.chat-top-operBtn {
		padding: 0 !important;
		font-size: 24rpx;
		border: none !important;
	}
	.oper-icon {
		width: 32rpx;
		height: 32rpx;
	}
	
	.chat-content {
		flex: 1;
		padding: 20rpx 20rpx 200rpx;
		background-color: #f5f7fb;
		overflow-x: hidden;
		overflow-y: auto;
		font-size: 28rpx;
	}
	.chat-content-tips {
		padding-right: 80rpx;
	}
	.chat-content-tips-title {
		margin: 8rpx 0;
	}
	.chat-content-tips-list-li {
		display: inline-block;
		margin: 8rpx 0;
		padding: 12rpx 20rpx;
		background-color: #cce5ff;
		border-radius: 12rpx;
	}
	.chat-content-text {
	}
	.chat-text-li {
		margin-top: 40rpx;
	}
	.chat-text-AI {
		padding-right: 80rpx;
	}
	.chat-text-AI .chat-title {
		margin: 12rpx 0;
	}
	.chat-text-AI .chat-textValue {
		display: inline-block;
		margin: 8rpx 0;
		padding: 12rpx 20rpx;
		border-radius: 12rpx;
		white-space: normal;
		background-color: #ffffff;
	}
	.chat-text-P {
		padding-left: 80rpx;
		text-align: right;
	}
	.chat-text-P .chat-title {
		margin: 12rpx 0;
	}
	.chat-text-P .chat-textValue {
		display: inline-block;
		margin: 8rpx 0;
		padding: 12rpx 20rpx;
		border-radius: 12rpx;
		color: #ffffff;
		white-space: normal;
		background-color: #25a3ff;
	}
	.chat-bottom {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 12rpx 20rpx;
	}
	.bottom-input {
		flex: 1;
		padding: 12rpx 20rpx;
		font-size: 28rpx;
		border-radius: 12rpx;
		background-color: #f7f7f7;
	}
	.submit-btn {
		padding: 0 96rpx;
		margin: 0 0 0 20rpx !important;
		font-size: 28rpx;
		color: #ffffff;
		background-color: #23a3ff !important;
	}
	.key-board-height {
		position: fixed;
		left: 0;
		right: 0;
		bottom: 20rpx;
		padding: 12rpx 20rpx;
		background-color: #ffffff;
	}
</style>
