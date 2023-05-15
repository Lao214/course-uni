<template>
	<view class="container">
		<view class="header">
			<h1 style="margin-right: 30rpx;color: #2c30306e;" @click="gotoNotes()">笔记</h1>
			<h1>待办事项</h1>
		</view>
		<view class="body" v-for="(item,index) in list" :key="index">
			<view class="item-header" @click="toggleCollapse(item)" @change="handleItemChange">
				<label style="width: 80%;" @click.stop="chooseAll(item)">
					<checkbox :checked="item.done" />
					{{ item.title }}
				</label>
				<text style="width: 10%;" >{{ item.checkedCount }}/{{item.subList.length}}</text>
				<text style="width: 10%;text-align: center;">▼</text>
			</view>
			<transition name="fade">
				<view v-show="item.isCollapsed" style="margin-left: 50rpx;">
					<view v-for="(subItem,subIndex) in item.subList" :key="subIndex" style="margin: 5px;">
						<label @click="chooseSingle(item,subItem)">
							<checkbox :checked="subItem.done" />
							{{ subItem.title }}
						</label>
					</view>
				</view>
			</transition>
			<view style="width: 100%;display: flex;justify-content: center;font-size: 20px;">
				<text @click="addTask(index)" style="width: 20px;height: 20px;border: 2px solid silver;text-align: center;line-height: 20px;">
					+
				</text>
			</view>
		</view>
		<view v-show="showOne" style="position: absolute;width: 100%;height: 100%;background-color: #2c30306e;top: 0%;">
			<view class="dialog">
				<text>细项：</text>
				<input type="text" v-model="inputValue" style="width: 77%;border-bottom: #2c30306e 2px solid;" />
				<view class="btn" @click="showOne = false">
					<span>取消</span>
				</view>
				<view class="btn" @click="confirmAddTask()">
					<span>确定</span>
				</view>
			</view>
		</view>
		<view v-show="showTwo" style="position: absolute;width: 100%;height: 100%;background-color: #2c30306e;top: 0%;">
			<view class="dialog">
				<text>事项：</text>
				<input type="text" v-model="inputValue2" style="width: 77%;border-bottom: #2c30306e 2px solid;" />
				<view class="btn" @click="showTwo = false">
					<span>取消</span>
				</view>
				<view class="btn" @click="confirmAddMatter()">
					<span>确定</span>
				</view>
			</view>
		</view>
		<view class="addSign" @click="showTwo = true">
			<span style="font-size: 70rpx;font-weight: 900;color: white;">+</span>
		</view>
	</view>
</template>

<script>
export default {
  data() {
    return {
		showOne: false,
		showTwo: false,
		showItem: 0,
		inputValue: '',
		inputValue2: '',
		list: []
    }
  },
  onLoad() {
  	this.getData()
  },
  methods: {
	setData() {
		// 存储数据
		uni.setStorage({
			key: 'list',
			data: this.list,
			success: function () {
				console.log('数据存储成功')
			}
		})
	},
	getData() {
		// 获取数据
		uni.getStorage({
			key: 'list',
			success: (res) => {
				// 将获取到的数据赋值给Vue实例的data属性中的变量
				this.list = res.data
			}
		})
	},
	gotoNotes() {
		uni.navigateTo({
			url: '/pages/notes/notes'
		})
	},
	toggleCollapse(item) {
		if (item.subList && item.subList.length > 0) {
		    item.isCollapsed = !item.isCollapsed
		}
		this.setData()
	},
	chooseAll(item) {
		if (item.subList && item.subList.length > 0) {
		    item.isCollapsed = true
		}
		if(item.done === false) {
			item.subList.forEach((element) => {
				element.done = true
			})
			item.checkedCount = item.subList.length
			item.done = true
		} else if(item.done === true) {
			item.subList.forEach((element) => {
				element.done = false
			})
			item.checkedCount = 0
			item.done = false
		}
		this.setData()
	},
	chooseSingle(item,subItem) {
		subItem.done = !subItem.done
		if(subItem.done) {
			item.checkedCount++
		} else if (!subItem.done) {
			item.checkedCount--
		}
		if(item.checkedCount === 0) {
			item.done = false
		} else if(item.checkedCount === item.subList.length) {
			item.done = true
		}
		this.setData()
	},
	addTask(index) {
		this.showOne = true
		this.showItem = index
	},
	confirmAddTask() {
		this.list[this.showItem].subList.push({
		    title: this.inputValue,
		    done: false
		})
		this.showOne = false
		this.list[this.showItem].isCollapsed = true
		uni.showToast({
		  title: '添加成功',
		  icon: 'success',
		  duration: 2000
		})
		this.setData()
	},
	confirmAddMatter() {
		this.list.push({
		    title: this.inputValue2,
		    done: false,
			checkedCount: 0,
		    subList: [],
		    isCollapsed: false
		})
		this.showTwo = false
		uni.showToast({
		  title: '添加成功',
		  icon: 'success',
		  duration: 2000
		})
		this.setData()
	}
  }
}
</script>

<style>
.addSign {
	width: 90rpx;
	height: 90rpx;
	background-color: #f5c243;
	text-align: center;
	line-height: 90rpx;
	border-radius: 50%;
	position: fixed;
	bottom: 5%;
	right: 5%;
}
.btn {
	border: 2px sandybrown solid;
	padding: 10rpx 40rpx;
	margin: 0 10rpx;
	height: 20%;
}
.dialog {
	background-color: white;
	min-height: 170px;
	border-radius: 15px;
	padding: 15px;
	width: 20em;
	position: absolute;
	left: 50%;
	top: 30%;
	transform: translateX(-50%);
	display: flex;
	flex-wrap: wrap;
	justify-content: center;
}
.item-header {
	width: auto;
	display: flex;
	margin: 15px 10px;
	justify-content: space-between;
	cursor: pointer;
}
.body {
	background: white;
	margin: 15px;
	padding: 15px;
	border-radius: 15px;
	min-height: 170px;
}	

.header {
	display: flex;
	padding-left: 15px;
	padding-top: 15px;
	font-family: 'Courier New', Courier, monospace;
}

page {
	background-color: #efeef1;
}
ul, ol {
  list-style-type: none;
}
.fade-enter-active {
	animation: scale-in-ver-top 0.4s cubic-bezier(0.250, 0.460, 0.450, 0.940) both;
}

.fade-leave-active {
	animation: scale-in-ver-top 0.4s cubic-bezier(0.250, 0.460, 0.450, 0.940) reverse;
}

@keyframes scale-in-ver-top {
	0% {
     transform: translateY(-20px);
     opacity: 0;
   }
   100% {
     transform: translateY(0);
     opacity: 1;
   }
}
</style>
