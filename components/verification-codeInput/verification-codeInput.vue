<template>
	<view class="verifical-input">
		<input
			class="verifical-input-hidden"
			type="number" 
			:maxlength="blockNum"
			:focus="isFucus"
			hold-keyboard="true"
			@input = "changeCodeInput"
			v-model="inputCode"
			value="" />
		<view class="verifical-input-real">
			<block v-for="(item,index) in blockNum" :key="index">
				<view :class="['real-block',{'block-active':index === activeIndex && codeType == 'block','line-active':index === activeIndex && codeType == 'line','block-arror':errorType},codeType == 'block'?'block-content':'line-content']">
					<text class="real-block-line" v-if="index === activeIndex || (errorType && index === 0)"></text>
					<text class="real-block-number">{{inputText[index]}}</text>
				</view>
			</block>	
		</view>
	</view>
</template>

<script>
	export default {
		props:{
			//验证码个数
			blockNum:{
				type:Number,
				default:4
			},
			//验证码类型
			codeType:{
				type:String,
				default:'block'
			},
			/* isFocus:{
				type:Number,
				default:4
			} */
		},
		data() {
			return {
				activeIndex:0, //激活的方块
				inputText:'',  //输入的验证码
				isFucus:true,  //是否自动聚焦
				inputCode:'',  //输入的值
				errorType:false //错误提示
			}
		},
		watch: {
			errorType: {
				immediate: true,
				handler: function(newValue) {
					if (newValue === true) {
						this.inputText.length = 0;
						this.inputCode = '';
						this.isFucus = true;
					}
				}
			}
		},
		methods: {
			changeCodeInput(event){
				this.errorType = false;
				this.inputText = (event.target.value).split('');
				this.activeIndex = this.inputText.length;
				if(this.activeIndex == this.blockNum){
					this.isFucus = false;
					this.$emit('verificationCode',event.target.value)
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	$main-color:blue;/* 主题色  */
	$error-color:red;/* 错误颜色  */
	.flex-row{
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}
	.flex-column{
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
	.verifical-input{
		position: relative;
		overflow: hidden;
		padding: 0 5px;
		.verifical-input-hidden{
			position: absolute;
			top:0;
			left:-200%;
			width: 300%;
			height: 100%;
			background: none;
			color: #FFFFFF;
		}
		.verifical-input-real{
			width: 100%;
			@extend  .flex-row;
			.real-block{
				width: 100rpx;
				height: 100rpx;
				margin-right: 20rpx;				
				&:last-child{
					margin-right: 0;
				}		 		
				@extend  .flex-row;
				.real-block-line{
					display: inline-block;
					width: 6rpx;
					height: 46rpx;
					background: #333333;
					animation: line 1s infinite ease;
				}
				.real-block-number{
					font-size: 48rpx;
					font-weight: 600;
				}
			}
			.line-content{
				border-bottom: 2rpx solid rgba(187, 187, 187, 100);
			}
			.block-content{
				border-radius: 12rpx;
				border: 2rpx solid rgba(187, 187, 187, 100);
			}
			.block-active{
				border: 4rpx solid $main-color!important;
			}
			.line-active{
				border-bottom: 2rpx solid $main-color;
			}
		}
	}
	/* 错误弹框 */
	.block-arror{
		border-color: $error-color!important;
		animation: error .5s ease;
	}
	@keyframes line {
		0% {
			opacity: .9;
		}
		50% {
			opacity: 0;
		}
		100% {
			opacity: .9;
		}
	}
	@keyframes error {
		0% {
			transform: translateX(-5px);
		}
		20% {
			transform: translateX(5px);
		}
		40% {
			transform: translateX(-5px);
		}
		60% {
			transform: translateX(5px);
		}
		80% {
			transform: translateX(-5px);
		}
		100% {
			transform: translateX(0);
		}
	}
</style>
