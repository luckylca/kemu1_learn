<template>
	<view class="container">
		<view class="layout">
			<view class="problem" id="problem">
				<image v-if="problem_img" :src="problem_img" mode="widthFix" id="image"></image>
				<view class="txt" id="txt">
					<view class="txtcontent">
						{{problem_text}}
					</view>
				</view>
				
				
			</view>
			<view class="choose" @click="choosea" id="a">
				<view class="letter">
					A
				</view>
				<view class="text">
					{{atext}}
				</view>
			</view>
			<view class="choose" @click="chooseb" id="b">
				<view class="letter">
					B
				</view>
				<view class="text">
					{{btext}}
				</view>				
			</view>
			<view class="choose" @click="choosec" id="c">
				<view class="letter">
					C
				</view>
				<view class="text">
					{{ctext}}
				</view>				
			</view>
			<view class="choose" @click="choosed" id="d"> 
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
				<uni-popup-message type="error" message="这是判断题!!!!!" :duration="2000"></uni-popup-message>
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
const atext=ref([])
const btext=ref([])
const ctext=ref([])
const dtext=ref([])
const problem_text=ref([])
const answer=ref([])
const ansys=ref([])
let score=0
const index=ref()
const count=0
let counts=0;
let finalcount=0;
let problems = []
const list=ref([])
let idlist=[]

watch(counts,(newdata)=>{
	if(newdata==finalcount)
	{
		uni.navigateTo({
			url:"/pages/complete/complete?mode=${myVariable}"
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
		url:"https://www.mxnzp.com/api/driver_exam/question/list?rank=1&type=1",
		data:{
			app_id:"yxjlfatjodamkgwq",
			app_secret:"FtIjzYCwso9zjledzyutH4Q3cpPZpKI8",
			page:Math.floor(Math.random() * 50) + 1
		},
	}).then(res=>{
		
		
		list.value=res.data.data.list
		idlist=list.value.map(({ id }) => id)

		// problem_img.value=res.data.data.list[index].titlePic;
		// atext.value=res.data.A;
		// btext.value=res.data.B;
		// ctext.value=res.data.C;
		// dtext.value=res.data.D;
		// problem_text.value=res.data.question
		// answer.value=res.data.key
		// ansys.value=res.data.analysis
	})
	uni.request({
		url:"https://www.mxnzp.com/api/driver_exam/answer/list",
		data:{
			app_id:"yxjlfatjodamkgwq",
			app_secret:"FtIjzYCwso9zjledzyutH4Q3cpPZpKI8",
			ids:JSON.stringify(idlist).slice(1, -1)
		},
	}).then((res)=>{
		
		console.log(res);
		
	}
)
	
	if(problem_img.value=="")
	{
		document.getElementById("problem").classList.remove("has_image")
	}
	else{
		document.getElementById("problem").classList.add("has_image")
	}
	document.getElementById("txt").style.display="flex"
}


function choosea(){
	document.getElementById("a").classList.add("ani")
	document.getElementById("a").addEventListener("animationend", ()=>{
		document.getElementById("a").classList.remove("ani")
	},{once:true})
	if(answer.value=="a")
	{
		get_problem()
		score++;
	}
	else
	{	
		let problem = new Problem(problem_img.value,problem_text.value,atext.value,btext.value,ctext.value,dtext.value,answer.value,ansys.value,count)
		problems.push(problem)
		popupRef.value.open('center')
		
	}
	counts++;
}
function chooseb(){
	document.getElementById("b").classList.add("ani")
	document.getElementById("b").addEventListener("animationend", ()=>{
		document.getElementById("b").classList.remove("ani")
	},{once:true})
	if(answer.value=="b")
	{
		get_problem()
		score++;
	}
	else
	{
		let problem = new Problem(problem_img.value,problem_text.value,atext.value,btext.value,ctext.value,dtext.value,answer.value,ansys.value,count)
		problems.push(problem)
		popupRef.value.open('center')
	}
	counts++;
	
	
}
function choosec(){
	document.getElementById("c").classList.add("ani")
	document.getElementById("c").addEventListener("animationend", ()=>{
		document.getElementById("c").classList.remove("ani")
	},{once:true})
	if(ctext.value==""){
		popup1.value.open()
	}
	else{
			if(answer.value=="c")
		{
			get_problem()
			score++;
		}
		else
		{
			let problem = new Problem(problem_img.value,problem_text.value,atext.value,btext.value,ctext.value,dtext.value,answer.value,ansys.value,count)
			problems.push(problem)
			popupRef.value.open('center')
		}
		counts++;
	}

	
	
	
}
function choosed(){
	document.getElementById("d").classList.add("ani")
	document.getElementById("d").addEventListener("animationend", ()=>{
		document.getElementById("d").classList.remove("ani")
	},{once:true})
	if(dtext.value=="")
	{
		popup1.value.open()
	}
	else
	{
			if(answer.value=="d")
		{
			get_problem()
			score++;
		}
		else
		{
			let problem = new Problem(problem_img.value,problem_text.value,atext.value,btext.value,ctext.value,dtext.value,answer.value,ansys.value,count)
			problems.push(problem)
			popupRef.value.open('center')
		}
		counts++;
	}

	
	
	
}
onMounted(()=>{
	uni.getStorage({
		key:"mode",
		success: function (res) {
			if(res.data==1)
			{
				finalcount=10
			}
			else if(res.data==2)
			{
				finalcount=100
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
			border: 1px solid red;
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
				display: none;
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
			border: 1px solid black;
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