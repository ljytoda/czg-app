	<!-- 
	实现移动端的功能，顶部：左侧是一个圆形头像，中间显示名字，右侧是签到按钮。中间：有一个日历可以选择日期，右下部有一个按钮并且是一个圆形图标。底部有四个可点击图标，点击后会切换页面，并且图标会变色表示被激活，这四个图标下面分别名字叫“首页“，”监控“，”成长“，”宝宝“。上面的三部分之间用一条灰色的线条隔开 
	-->
	<template>
		<view class="container">
			<!-- 顶部 -->
			<view class="top">
				<view class="avatar"></view>
				<view class="name">用户名</view>
				<view class="signin">签到></view>
			</view>
			<!-- 中间 -->
			<view class="middle">
			  <calendar :default-date="defaultDate" @change="onDateChange" @open="toggleCalendar" @close="toggleCalendar"></calendar>

			  <view class="middle-a">
			    <button class="button" :class="{ hidden: calendarOpen }" @click="onButtonClick"></button>
			  </view>
			</view>

			
			<!-- 底部 -->
			<view class="bottom">
				<view class="item" :class="{active: activeIndex === 0}" @click="changeTab(0)">
					<image class="icon" src="@/static/footer/home.png"></image>
					<view class="text">首页</view>
				</view>
				<view class="item" :class="{active: activeIndex === 1}" @click="changeTab(1)">
					<image class="icon" src="@/static/footer/monitor.png"></image>
					<view class="text">监控</view>
				</view>
				<view class="item" :class="{active: activeIndex === 2}" @click="changeTab(2)">
					<image class="icon" src="@/static/footer/growth1.png"></image>
					<view class="text">成长</view>
				</view>
				<view class="item" :class="{active: activeIndex === 3}" @click="changeTab(3)">
					<image class="icon" src="@/static/footer/baby.png"></image>
					<view class="text">宝宝</view>
				</view>
			</view>
		</view>
	</template>


	<script>
		import calendar from '@/components/calender.vue'
		// import button from '@/components/button.vue'

		export default {
			components: {
				calendar,
				// button
			},
			data() {
			  return {
			    activeIndex: 0,
			    defaultDate: new Date(),
			    calendarOpen: false
			  }
			},
			methods: {
			  changeTab(index) {
			    var link = '';
			    if (index===1) {
			      link = '/pages/monitor/index'
			    } else if(index===2){
			      link = '/pages/growth/index'
			    } else if(index===3){
			      link = '/pages/baby/index'
			    }
			    uni.navigateTo({
			      url: link,
			      success: res => {
			        // 成功跳转后的回调函数
			        console.log('跳转成功')
			      },
			      fail: err => {
			        // 跳转失败后的回调函数
			        console.log('跳转失败', err)
			      }
			    })
			  }, 
			  onDateChange(date) {
			    console.log('选择的日期为：', date)
			    this.calendarOpen = false
			  },
			  onButtonClick() {
			    console.log('按钮被点击了')
			  },
			  toggleCalendar() {
			    this.calendarOpen = !this.calendarOpen
			  }
			}

		}
	</script>


	<style>
		.container {
			display: flex;
			flex-direction: column;
			height: 100vh;
		}

		.top {
			display: flex;
			justify-content: space-between;
			align-items: center;
			height: 10%;
			padding: 0 0px;
			background-color: #DDEEFF;
			border-bottom: 1px solid #ccc;
		}

		.avatar {
			width: 45px;
			height: 60%;
			border-radius: 50%;
			background-image: url('@/static/av.png');
			background-size: contain;
		}

		.name {
			font-size: 18px;
			font-weight: bold;
			margin-left: 20px;
		}

		.signin {
			font-size: 16px;
			color: #000000;
			/* background-color: #ffaaff; */
			padding: 10px 10px;
		}

		.middle {
			flex: 1;
			display: flex;
			height: 60%;
			flex-direction: column;
			align-items: center;
			border-bottom: 1px solid #ccc;
		}

		.middle-a {
		  position: absolute;
		  bottom: 0;
		  left: 80%;
		  right: 0;
		  display: flex;
		  height: 20%;
		  flex-direction: column;
		  align-items: center;
		  /* border: 1px solid #ff00ff; */
		}
		.middle-a .hidden {
		  display: none;
		}


		.calendar {
			width: 100%;
			height: 300px;
			background-color: #ffffff;
			margin-bottom: 20px;
			display: flex;
			flex-direction: column;
			overflow: hidden;
			transition: height 0.3s linear;
		}

		.button {
			width: 40px;
			height: 40px;
			/* border-radius: 50%; */
			background-image: url('@/static/edit.png');
			background-size: contain;
			filter: contrast(175%)opacity(75%);
		}

		.bottom {
			display: flex;
			height: 10%;
			justify-content: space-around;
			align-items: center;
			/* height: 80px; */
			background-color: #fff;
		}

		.item {
			display: flex;
			flex-direction: column;
			align-items: center;
		}

		.icon {
			width: 30px;
			height: 30px;
		}

		.text {
			font-size: 14px;
			margin-top: 0px;
		}

		.item.active .icon {
			filter: invert(20%) sepia(100%) saturate(1000%) hue-rotate(180deg) brightness(109%) contrast(107%);
		}

		.item.active .text {
			color: #6cd8ff;
		}
	</style>