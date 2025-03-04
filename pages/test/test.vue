<template>
	<view class="container">
		<view class="layout">
			<view class="problem" id="problem" :class="{has_image:has_img===true}">
				<image v-if="problem_img" :src="problem_img" mode="widthFix" id="image"></image>
				<view class="txt" id="txt" :class="{display1:isdisplay===true}">
					<view class="txtcontent">
						{{problem_text}}
					</view>
				</view>
				
				
			</view>
			<view class="choose" @click="choosea" id="a" :class="{ani:isani1===true}">
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
			<uni-popup ref="popup1" type="message">
				<uni-popup-message type="error" message="请不要选空!!!!!" :duration="2000"></uni-popup-message>
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
let score=0
const count=ref()
const counts=ref(0);
const finalcount=ref(0);
let problems = []
const isjudge=ref(false)
const has_img=ref()
const isdisplay=ref(true)
const isani1=ref()
const isani2=ref()
const isani3=ref()
const isani4=ref()

watch(counts,(newdata)=>{
	if(newdata==finalcount.value)
	{
		uni.redirectTo({
			url:`/pages/complete/complete?finalcount=${finalcount.value}&score=${score}`
		})
	}
})

function changepoup(res){
	
	if(res.show==false)
	{
		get_problem()
	}
	
}

function Problem(problem_img,problem_text,atext,btext,ctext,dtext,answer,ansys,count){
	this.problem_img=problem_img
	this.atext=atext
	this.btext=btext
	this.ctext=ctext
	this.dtext=dtext
	this.problem_text=problem_text
	this.answer=answer
	this.ansys=ansys
	this.count=count
}

function get_problem(){
		uni.request({
		url:"https://mnks.jxedt.com/get_question",
		data:{
			index:Math.floor(Math.random() * 3933) + 1
		}
	}).then(res=>{
		problem_img.value=res.data.imageurl;
		atext.value=res.data.a;
		btext.value=res.data.b;
		ctext.value=res.data.c;
		dtext.value=res.data.d;
		problem_text.value=res.data.question
		answer.value=res.data.ta
		ansys.value=res.data.bestanswer
		if(atext.value==""&&btext.value==""&&ctext.value==""&&dtext.value=="")
		{
			atext.value="正确"
			btext.value="错误"
		}
	})
	if(problem_img.value=="")
	{
		has_img.value=true
	}
	else{
		has_img.value=false
	}

}


function choosea(){
	// document.getElementById("a").classList.add("ani")
	// document.getElementById("a").addEventListener("animationend", ()=>{
	// 	document.getElementById("a").classList.remove("ani")
	// },{once:true})
	isani1.value=true
	setTimeout(()=>{
		isani1.value=false	
		
	},300)
	if(answer.value=="1")
	{
		get_problem()
		score++;
	}
	else
	{	
		let problem = new Problem(problem_img.value,problem_text.value,atext.value,btext.value,ctext.value,dtext.value,answer.value,ansys.value,count.value)
		problems.push(problem)
		uni.setStorage({
			key:"problems",
			data:problems
		})
		popupRef.value.open('center')
		
	}
	counts.value++;
}
function chooseb(){
	// document.getElementById("b").classList.add("ani")
	// document.getElementById("b").addEventListener("animationend", ()=>{
	// 	document.getElementById("b").classList.remove("ani")
	// },{once:true})
	isani2.value=true
	setTimeout(()=>{
		isani2.value=false	
		
	},300)
	if(answer.value=="2")
	{
		get_problem()
		score++;
	}
	else
	{
		let problem = new Problem(problem_img.value,problem_text.value,atext.value,btext.value,ctext.value,dtext.value,answer.value,ansys.value,count.value)
		problems.push(problem)
		uni.setStorage({
			key:"problems",
			data:problems
		})
		popupRef.value.open('center')
	}
	counts.value++;
	
}
function choosec(){
	// document.getElementById("c").classList.add("ani")
	// document.getElementById("c").addEventListener("animationend", ()=>{
	// 	document.getElementById("c").classList.remove("ani")
	// },{once:true})
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
			get_problem()
			score++;
		}
		else
		{
			let problem = new Problem(problem_img.value,problem_text.value,atext.value,btext.value,ctext.value,dtext.value,answer.value,ansys.value,count.value)
			problems.push(problem)
			uni.setStorage({
				key:"problems",
				data:problems
			})
			popupRef.value.open('center')
		}
		counts.value++;
	}

	
	
	
}
function choosed(){
	// document.getElementById("d").classList.add("ani")
	// document.getElementById("d").addEventListener("animationend", ()=>{
	// 	document.getElementById("d").classList.remove("ani")
	// },{once:true})
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
			get_problem()
			score++;
		}
		else
		{
			let problem = new Problem(problem_img.value,problem_text.value,atext.value,btext.value,ctext.value,dtext.value,answer.value,ansys.value,count.value)
			problems.push(problem)
			uni.setStorage({
				key:"problems",
				data:problems
			})
			popupRef.value.open('center')
		}
		counts.value++;
	}

	
	
	
}
onMounted(()=>{
	uni.getStorage({
		key:"mode",
		success: function (res) {
			if(res.data==1)
			{
				finalcount.value=10
			}
			else if(res.data==2)
			{
				finalcount.value=100
			}
		}
	})
	get_problem()
	uni.getStorage({
		key:"problems",
		success: function(res){
			problems=res.data
		}
	})
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
				padding-left: 20rpx;
				padding-right: 20rpx;
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