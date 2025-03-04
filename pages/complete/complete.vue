<template>
	<view>
		<view class="img">
			<uni-icons type="checkmarkempty" size="200" color="green" class=""></uni-icons>
		</view>

		<view class="txt">
			<text>{{ displayText }}<text v-if="showCursor">|</text></text>
		</view>
		<view class="back" @click="back">
			返回首页
		</view>
	</view>
</template>

<script setup>
import { onMounted, onUnmounted, ref } from 'vue';
import {onLoad} from "@dcloudio/uni-app"


const allcount=ref(null)
const score=ref(null)

const displayText = ref(""); // 当前显示的文本（响应式）
const showCursor = ref(true); // 控制光标显隐（响应式）
let timer = null; // 定时器变量
let index = 0; // 当前显示的字符索引

// 逐字显示函数
function startTyping(text) {
  const fullText = decodeURIComponent(text); // 解码参数 <button class="citation-flag" data-index="5">
  timer = setInterval(() => {
    if (index <= fullText.length) {
      displayText.value = fullText.substr(0, index);
      index++;
    } else {
      clearInterval(timer);
      showCursor.value = false;
    }
  }, 170); // 调整速度（单位：毫秒）
}

// 光标闪烁函数
function toggleCursor() {
  setInterval(() => {
    showCursor.value = !showCursor.value; // 切换光标状态
  }, 500); // 每500ms闪烁一次
}

function back(){
	uni.setStorage({
		key:"mode",
		data:0
	})
	uni.redirectTo({
		url:"/pages/index/index"
	})
	
}
	
	
	
onLoad((res)=>{
	allcount.value =decodeURIComponent(res.finalcount || "");
	score.value = decodeURIComponent(res.score || "");
	
})

onMounted(()=>{	
	if(allcount.value=="10")
	score.value=score.value*10;

	const text="你的分数是："
	const fullText = text + score.value; // 完整文本
	startTyping(fullText)
	toggleCursor()

	
	
})

onUnmounted(()=>{
	clearInterval(timer);
})
</script>

<style lang="scss" scoped>
	.img{
		margin-top: 300rpx;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.txt{
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 45rpx;
		margin-top: 40rpx;
		
	}
	// .back{
	// 	font-size: 35rpx;
	// 	border: 1px solid gray;
	// 	width: 400rpx;
	// 	margin: 10px auto;
	// 	display: flex;
	// 	align-items: center;
	// 	justify-content: center;
	// 	border-radius: ;
		
	// }
		.back {
			width: 300rpx;
			padding: 25px;
			margin: 50px auto;
			background: linear-gradient(135deg, #f8f9fa, #e9ecef);
			border-radius: 15px;
			box-shadow: 0 10px 20px rgba(0,0,0,0.1);
			color: #333;
			font-family: Arial, sans-serif;
			transition: transform 0.3s, box-shadow 0.3s;
		}

		.back:hover {
			transform: translateY(-5px);
			box-shadow: 0 15px 30px rgba(0,0,0,0.15);
		} 
</style>
