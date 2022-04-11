<template>
	<view >
<!-- 
		<view class="intro">本项目已包含uni ui组件，无需import和注册，可直接使用。在代码区键入字母u，即可通过代码助手列出所有可用组件。光标置于组件名称处按F1，即可查看组件文档。</view>
		<text class="intro">详见：</text>
		<uni-link :href="href" :text="href"></uni-link> -->
		
		
		<uni-card title="游戏介绍" extra="管你看没看懂玩就完了">
			<text>游戏的规则非常的简单啊，点击下方按钮选择难度，进入页面后会随机生成一组数字，只需猜出数字就可获胜，根据你输入的数字，系统会给出相应的提示，根据提示再去猜正确的数字，直到最后结束（注：每次生成的数字没有重复的）</text>
		</uni-card>
			<uni-card title="选择难度" extra="点击按钮进入游戏">
					<view class="example-body">
						<uni-row class="demo-uni-row" :gutter="gutter" :width="nvueWidth">
							<uni-col :span="8">
								<button type="default" @click="goto('/pages/guess/guess?level=3')">简单</button>
							</uni-col>
						
							<uni-col :span="8">
								<button type="warn" @click="goto('/pages/guess/guess?level=5')">中等</button>
							</uni-col>
							
							<uni-col :span="8">
								<button type="primary" @click="goto('/pages/guess/guess?level=7')">复杂</button>
							</uni-col>
						</uni-row>
					</view>
				</uni-card>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				href: 'https://uniapp.dcloud.io/component/README?id=uniui'
			}
		},
		methods: {

			goto(url) {
				console.log("按钮被点击了，且传入的参数是：" + url)
				uni.navigateTo({
					url: url
				});
			}
		}
	}
</script>

<style>
	.container {
		padding: 20px;
		font-size: 14px;
		line-height: 24px;
	}
</style>
