<template>
	<view>
		<fireworks v-if="isReady" />
		<!-- <codeInput ref="codeInput" @verificationCode="verificationCode" codeType="line" :errorType="errorType" :blockNum="blockNum"></codeInput> -->
		<uni-card>
			<codeInput ref="codeInput" @verificationCode="verificationCode" codeType="line" :blockNum="blockNum">
			</codeInput>
			<uni-list :border="false">
				<uni-list-item title="我的猜想" />

				<uni-list-item :border="false" v-for="think in thinkList" :key="think.num" :title="think.num"
					:note="think.note" rightText="" />
			</uni-list>
		</uni-card>
	</view>
</template>

<script>
	import codeInput from '@/components/verification-codeInput/verification-codeInput.vue'
	import fireworks from '@/components/sanshui-fireworks/sanshui-fireworks.vue'
	export default {
		components: {
			codeInput,
			fireworks
		},

		data() {
			return {
				//放花
				isReady: false,
				inputList: [],
				resultNum: [],
				blockNum: 2,
				thinkList: []
				// thinkList:[{num:3947,note:"4个号码正确,4个位置正确"},{num:3947,note:"4个号码正确,4个位置正确"},{num:3947,note:"4个号码正确,4个位置正确"},{num:3947,note:"4个号码正确,4个位置正确"}]
			}
		},

		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数
			// console.log(option.level); //打印出上个页面传递的参数。
			this.blockNum = parseInt(option.level);
			//生成一个不重复的随机level个的数组
			this.getNum(0);
			 // console.log(this.resultNum);
		},

		methods: {
			verificationCode(thinkNum) {
				setTimeout(() => {

					//进行对比 然后赋值
					this.inputList = this.$refs.codeInput.inputText.toString().split(",");
					//输入正确的数字个数
					var cNumber = 0;
					//输入正确的位置个数
					var locNumber = 0;
					//判断相同的元素
					var sameInputList =[];
					console.log("inputList", this.inputList)
					for (var lindex = 0; lindex < this.inputList.length; lindex++) {
						// console.log("locNumber",this.inputList[lindex] , this.resultNum[lindex],this.inputList[lindex] == this.resultNum[lindex])
						if (this.inputList[lindex] == this.resultNum[lindex]) {
							locNumber++;
						}
						for (var nindex = 0; nindex < this.resultNum.length; nindex++) {
							// console.log("cNumber",this.inputList[nindex] , this.resultNum[nindex],this.inputList[nindex] == this.resultNum[nindex])
							
							
							
						
							if (this.inputList[lindex] == this.resultNum[nindex]) {
									
								if(sameInputList.lastIndexOf(this.inputList[lindex]) < 0){
									cNumber++;
									sameInputList.push(this.inputList[lindex]);
								}
								
							}
						}
					}

					this.thinkList.unshift({
						num: thinkNum,
						note: cNumber + "个号码正确," + locNumber + "个位置正确"
					})

					if (cNumber == this.inputList.length && locNumber == this.inputList.length) {
						this.isReady = true;
						setTimeout(() => {
							this.isReady = false;
							this.thinkList = [];
							this.resultNum = [];
							this.getNum(0);
						}, 1000);
					}
					this.$refs.codeInput.activeIndex = 0 //激活的方块
					this.$refs.codeInput.inputText = '' //输入的验证码
					this.$refs.codeInput.isFucus = true //是否自动聚焦
					this.$refs.codeInput.inputCode = '' //输入的值
				}, 1000);
			},
			getNum(index) {
				var cNum = Math.floor(Math.random() * 10);

				if (index < this.blockNum) {
					if (this.resultNum.lastIndexOf(cNum) > -1) {
						this.getNum(index);
					} else {
						this.resultNum.push(cNum);
						this.getNum(index + 1);
					}
				} else {
					return;
				}
			}
		}
	}
</script>

<style>
</style>
