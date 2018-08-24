<template>
	<view>
		<view class="padding"></view>
		<view class="content">
			<view class="nav">
				<view class="nav-left">
					<scroll-view scroll-y :style="'height:'+height+'px'">
						<view class="nav-left-item" @click="categoryClickMain(item,index)" :key="index" :class="index==categoryActive?'active':''" v-for="(item,index) in categoryList">
							{{item.NAME}}
						</view>
					</scroll-view>
				</view>
				<view class="nav-right">
					<scroll-view scroll-y :scroll-top="scrollTop" @scroll="scroll" :style="'height:'+height+'px'" scroll-with-animation >
						<view :id="index==0?'first':''" class="nav-right-item" v-for="item in subCategoryList" :key="item">
							<image :src="item.LOGO" />
							<view>{{item.NAME}}</view>
						</view>
					</scroll-view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data:{
			categoryList:[],
			subCategoryList: [],
			height:0,
			categoryActive:0,
			scrollTop:0,
			scrollHeight:0
		},
		methods:{
			scroll(e){
				this.scrollHeight = e.detail.scrollHeight;
			},
			categoryClickMain(categroy, index) {
				this.categoryActive = index;
				this.subCategoryList = categroy.subCategoryList;
				this.scrollTop = -this.scrollHeight*index;
			},
			getCategory() {
				for(var i=0;i<20;i++){
					var subList = [];
					for(var j=0;j<30;j++){
						subList.push({"NAME":"分类"+i+":商品"+j,"LOGO":"http://placehold.it/50x50"})
					}
					this.categoryList.push({"NAME":"分类"+i,"subCategoryList":subList})
				}
				this.subCategoryList = this.categoryList[0].subCategoryList;
			}
		},
		onLoad:function(){
			this.getCategory()
			uni.getSystemInfo({
				success: res => {
					this.height = res.screenHeight;
				}
			})
		}
	}
</script>

<style>
	.nav {
		display: flex;
		width: 100%;
	}

	.nav-left {
		width: 30%;
	}

	.nav-left-item {
		height: 100px;
		border-right: solid 1px #E0E0E0;
		border-bottom: solid 1px #E0E0E0;
		font-size: 30px;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.nav-right {
		width: 70%;
		padding-top: 22px;
	}

	.nav-right-item {
		width: 28%;
		height: 220px;
		float: left;
		text-align: center;
		padding: 11px;
		font-size: 28px;
	}
	.nav-right-item image{
		width: 100px;
		height: 100px;
	}
	.active {
		color: #F24544;
	}

	.padding {
		height: var(--status-bar-height);
		width: 100%;
		top: 0;
		position: fixed;
		background-color: #F24544;
	}

	.content {
		padding-top: var(--status-bar-height);
		width: 100%;
	}
</style>
