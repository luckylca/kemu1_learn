<!-- <template>
<view class="container">
	<view class="layout">
		<view class="box" @click="daily" id="daily">
			每日一练
		</view>
		<view class="box" @click="test" id="test">
			全真模拟
		</view>
		<view class="box" @click="wrong" id="wrong">
			错题集
		</view>
	</view>
</view>
	
</template>

<script setup>
import {ref} from "vue"
import { onMounted } from "vue";

const daily1=ref(null)

function daily(){
	document.getElementById("daily").classList.add("ani")
	document.getElementById("daily").addEventListener("animationend", ()=>{
		document.getElementById("daily").classList.remove("ani")
	},{once:true})
	
	uni.setStorage({
		key:"mode",
		data:1
	})
	setTimeout(()=>{
		uni.navigateTo({
			url:"/pages/test/test"
		})		
	},300)

	
}

function test(){
	document.getElementById("test").classList.add("ani")
	document.getElementById("test").addEventListener("animationend", ()=>{
		document.getElementById("test").classList.remove("ani")
	},{once:true})
	uni.setStorage({
		key:"mode",
		data:2
	})
	setTimeout(()=>{
		uni.navigateTo({
			url:"/pages/test/test"
		})
	},300)

}
function wrong(){
	document.getElementById("wrong").classList.add("ani")
	document.getElementById("wrong").addEventListener("animationend", ()=>{
		document.getElementById("wrong").classList.remove("ani")
	},{once:true})
	uni.setStorage({
		key:"mode",
		data:3
	})	
	setTimeout(()=>{
		uni.navigateTo({
			url:"/pages/wrong/wrong"
		})
	},300)

}
function open(){
	
}
onMounted(()=>{
	  uni.createSelectorQuery()
		.select('#target')
		.boundingClientRect(res => {
		  console.log(res); // 获取布局信息 <button class="citation-flag" data-index="7">
		}).exec();
	uni.setStorage({
		key:"mode",
		data:0
	})
	uni.getStorage({
		key:"problems",
		success: function(res){
			
		},
		fail: function(res){
			uni.setStorage({
				key:"problems",
				data:[]
			})
		}
	})
	
})

</script>

<style lang="scss" scoped>
@keyframes anima
{
	0%{transform: scale(1);}
	25%{transform: scale(0.8);}
	75%{transform: scale(1.2);}
	100%{transform: scale(1);}
}
.ani{
	animation: anima 0.3s ease-in-out;
}
.container{
	.layout{
		padding-top: 20vh;
		.box{
			display: flex;
			align-items: center;
			justify-content: center;
			width: 570rpx;
			height: 100rpx;
			margin-left: auto;
			margin-right: auto;
			margin-bottom: 40px;
			font-size: 35rpx;
			border-radius: 30rpx;
			box-shadow: 
				0 2px 4px rgba(0,0,0,0.1),   /* 第一层：基础阴影 */
				0 6px 12px rgba(0,0,0,0.08), /* 第二层：中等模糊 */
				0 12px 24px rgba(0,0,0,0.06);/* 第三层：大范围扩散 */
			
		  /* 添加微立体效果 */
			transform: translateZ(0); /* 启用GPU加速 */
			  will-change: transform;   /* 优化渲染性能 */
			
		}
		
	}
}

</style -->>
<template>
  <view class="container">
    <view class="layout">
      <!-- 使用动态类名绑定 -->
      <view 
        class="box" 
        :class="{ ani: activeBox === 'daily' }" 
        @click="handleClick('daily', '/pages/test/test')"
        id="daily"
      >
        每日一练
      </view>
      <view 
        class="box" 
        :class="{ ani: activeBox === 'test' }" 
        @click="handleClick('test', '/pages/test/test')"
        id="test"
      >
        全真模拟
      </view>
      <view 
        class="box" 
        :class="{ ani: activeBox === 'wrong' }" 
        @click="handleClick('wrong', '/pages/wrong/wrong')"
        id="wrong"
      >
        错题集
      </view>
    </view>
  </view>
</template>

<script setup>
import { ref, onMounted } from "vue";

// 响应式变量控制动画状态
const activeBox = ref(null);


function handleClick(boxType, url) {
  activeBox.value = boxType;
  
  // 存储模式数据
  const mode = boxType === 'daily' ? 1 : boxType === 'test' ? 2 : 3;
  uni.setStorage({ key: "mode", data: mode });
  
  // 动画结束后跳转页面
  setTimeout(() => {
    activeBox.value = null;
    uni.navigateTo({ url });
  }, 300);
}

// 初始化存储
onMounted(() => {
  uni.setStorage({ key: "mode", data: 0 });
  
  // 初始化问题数据（如果不存在）
  uni.getStorage({
    key: "problems",
    success: (res) => console.log("Problems loaded:", res.data),
    fail: () => uni.setStorage({ key: "problems", data: [] })
  });
});
</script>

<style lang="scss" scoped>
@keyframes anima
{
	0%{transform: scale(1);}
	25%{transform: scale(0.8);}
	75%{transform: scale(1.2);}
	100%{transform: scale(1);}
}
.ani{
	animation: anima 0.3s ease-in-out;
}
.container{
	.layout{
		padding-top: 20vh;
		.box{
			display: flex;
			align-items: center;
			justify-content: center;
			width: 570rpx;
			height: 100rpx;
			margin-left: auto;
			margin-right: auto;
			margin-bottom: 40px;
			font-size: 35rpx;
			border-radius: 30rpx;
			box-shadow: 
				0 2px 4px rgba(0,0,0,0.1),   /* 第一层：基础阴影 */
				0 6px 12px rgba(0,0,0,0.08), /* 第二层：中等模糊 */
				0 12px 24px rgba(0,0,0,0.06);/* 第三层：大范围扩散 */
			
		  /* 添加微立体效果 */
			transform: translateZ(0); /* 启用GPU加速 */
			  will-change: transform;   /* 优化渲染性能 */
			
		}
		
	}
}

</style>
