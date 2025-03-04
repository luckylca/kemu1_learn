<template>
	<view class="container">
		<view class="layout">
			<view class="problem" id="problem" :class="{has_image:has_img===true}">
				<image v-if="problem_img" :src="problem_img" mode="widthFix" id="image"></image>
				<view class="txt" id="txt">
					<view class="txtcontent">
						{{problem_text}}({{count}}/3)
					</view>
				</view>
				
				
			</view>
			<view class="choose" @click="choosea" id="a"  :class="{ani:isani1===true}">
				<view class="letter">
					A
				</view>
				<view class="text">
					{{atext}}
				</view>
			</view>
			<view class="choose" @click="chooseb" id="b" :class="{ani:isani2===true}">
				<view class="letter">
					B
				</view>
				<view class="text">
					{{btext}}
				</view>				
			</view>
			<view class="choose" @click="choosec" id="c" :class="{ani:isani3===true}">
				<view class="letter">
					C
				</view>
				<view class="text">
					{{ctext}}
				</view>				
			</view>
			<view class="choose" @click="choosed" id="d" :class="{ani:isani4===true}"> 
				<view class="letter">
					D
				</view>
				<view class="text">
					{{dtext}}
				</view>				
			</view>
			<uni-popup ref="popupRef" type="center" @change="changepoup">
				<view class="backgrand">
					{{ansys}}
				</view>
			</uni-popup>
			<uni-popup ref="popup1" type="message" mask-click="false">
				<uni-popup-message type="error" message="暂时没有错题" :duration="2000"></uni-popup-message>
			</uni-popup>
			
		</view>
	</view>
</template>

<script setup>

import {onMounted, ref, watch} from "vue";
import uPopup from "@/uni_modules/uni-popup/components/uni-popup-dialog/uni-popup-dialog.vue"
import popup from "../../uni_modules/uni-popup/components/uni-popup/popup";
import completeVue from "../complete/complete.vue";
const popupRef = ref(null);
const popup1=ref(null);
const problem_img=ref("https://pic.52112.com/2020/04/13/JPG-200413_328/gCaPae4zjp_small.jpg")
const atext=ref("")
const btext=ref("")
const ctext=ref("")
const dtext=ref("")
const problem_text=ref("")
const answer=ref("")
const ansys=ref("")
let index=0;
const problems = ref([])
const count=ref(0)
const has_img=ref()

const isani1=ref()
const isani2=ref()
const isani3=ref()
const isani4=ref()



function changepoup(res){
	
	if(res.show==false)
	{
		get_problem()
	}
	
}


function get_problem(){
	console.log(problems.value[index]);
	atext.value=problems.value[index].atext
	btext.value=problems.value[index].btext
	ctext.value=problems.value[index].ctext
	dtext.value=problems.value[index].dtext
	problem_img.value=problems.value[index].problem_img
	problem_text.value=problems.value[index].problem_text
	answer.value=problems.value[index].answer
	ansys.value=problems.value[index].ansys
	count.value=problems.value[index].count
	if(count.value==null)
	{
		count.value=0
	}
	if(problem_img.value=="")
	{
		has_img.value=true
	}
	else{
		has_img.value=false
	}
	index++
	let all = problems.value.length
	if(index==all)
	index=0
}


function choosea(){
	isani1.value=true
	setTimeout(()=>{
		isani1.value=false	
	},300)
	if(answer.value=="1")
	{	
		problems.value[index].count++;
		count.value++;
		if(problems.value[index].count==3)
		{
			problems.value.splice(index,1)
		}
		get_problem()
		
	}
	else
	{	
		
		popupRef.value.open('center')
		
	}
}
function chooseb(){
	isani2.value=true
	setTimeout(()=>{
		isani2.value=false	
		
	},300)
	if(answer.value=="2")
	{	
		problems.value[index].count++;
		count.value++;
		if(problems.value[index].count==3)
		{
			problems.value.splice(index,1)
		}
		get_problem()
		
	}
	else
	{
		popupRef.value.open('center')
	}
	
}
function choosec(){
	isani3.value=true
	setTimeout(()=>{
		isani3.value=false	
		
	},300)
	if(ctext.value==""){
		popup1.value.open()
	}
	else{
			if(answer.value=="3")
		{	
			problems.value[index].count++
			count.value++;
			if(problems.value[index].count==3)
			{
				problems.value.splice(index,1)
			}
			get_problem()
			
		}
		else
		{
			popupRef.value.open('center')
		}
	}

	
	
	
}
function choosed(){
	isani4.value=true
	setTimeout(()=>{
		isani4.value=false	
	},300)
	if(dtext.value=="")
	{
		popup1.value.open()
	}
	else
	{
			if(answer.value=="4")
		{	
			problems.value[index].count++;
			count.value++;
			if(problems.value[index].count==3)
			{
				problems.value.splice(index,1)
			}
			get_problem()
			
		}
		else
		{
			popupRef.value.open('center')
		}

	}

	
	
	
}
onMounted(async ()=>{

	// uni.getStorage({
	// 	key:"problems",
	// 	success: function(res){
	// 		problems.value=res.data
	// 		console.log(problems.value);
	// 	}
	// })
	const res= await uni.getStorage({ key: "problems" });
	problems.value=res.data
	const filteredArr = problems.value.filter(
	  item => item && typeof item === "object" && Object.keys(item).length > 0
	);
	problems.value=filteredArr
	problems.value = problems.value.map(problem => ({
	...problem,
	count: problem.count || 0 // 确保 count 是数字
	}));
	if(problems.value==[])
	{
		popup1.value.open()
	}
	else
	{
		get_problem()		
	}

})

</script>

<style lang="less" scoped>
@keyframes anima {
	0%{transform: scale(1);}
	50%{transform: scale(1.1);}
	100%{transform: scale(1);}
}

.container{
	.layout{
		.problem{
			box-shadow: 0 2px 6px rgba(0,0,0,0.3);
			width: 600rpx;
			margin-left: auto;
			margin-right: auto;
			height: 50vh;
			margin-bottom: 20px;
			margin-top: 10px;
			border-radius: 30px;
			flex-direction: column;
			display: flex;
			justify-content: center;
			align-content: center;
			.txt{
				display: flex;
				justify-content: center;
				align-items: center;
			}
			.has_image .txtcontent{
				
				margin: auto auto;
			}
			

			image{
				width: 600rpx;
				border-radius: 30px;
				object-fit: contain;
			}
			

			
			
			
		}
		.choose{
			box-shadow: 0 2px 6px rgba(0,0,0,0.2);
			width: 600rpx;
			margin-left: auto;
			margin-right: auto;
			height: 5vh;
			border-radius: 30rpx;
			margin-bottom: 30rpx;
			position: relative;
			display: flex;
			align-items: center;				
			justify-content: center;
			.letter{
				position: absolute;
				left: 20rpx;
			}
			.text{
				font-size: 23rpx;
				max-width: 450rpx;
				overflow: hidden;
				
			}
		}
		.ani{
			animation:anima 0.3s ease-in-out;
		}
		.backgrand{
			display: flex;
			justify-content: center;
			align-items: center;
			padding-left: 50rpx;
			padding-right: 50rpx;
			background-color: white;
			width: 600rpx;
			height: 400px;
			box-sizing: border-box;
			border-radius: 40rpx;
		}
	}
}

</style>