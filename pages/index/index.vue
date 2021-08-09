<template>
	<view class="container">
		<image src="../../static/niunan.jpg" class="imgWapper"></image>
		<uni-row :gutter="10">
			<uni-col :span="6">
				<view :class="idName == item.id ? 'darkHighLight' : 'dark'" v-for="(item, index) in navName" @click="orderClick(item.id)" :key="index">{{ item.name }}</view>
			</uni-col>
			<uni-col :span="18">
				<scroll-view
					scroll-y="true"
					:style="{ height: navHeight + 'px' }"
					class="sv"
					:scroll-into-view="clickId"
					:scroll-with-animation="true"
					@scroll="scroll"
					@scrolltolower="scrolltolower"
				>
					<view v-for="(el, index) in navName" :key="index">
						<text style="color: #808080;"  class="nameTitle"   :id="'po' + index">{{ el.name }}</text>

						<view v-for="(item, index2) in el.list" :key="index2" class="wapper">
							<uni-row>
								<uni-col :span="6">
									<view style="text-align: center;"><image :src="item.img" class="orderInfo"></image></view>
								</uni-col>
								<uni-col :span="14">
									<view class="title" >{{ item.name }}</view>

									<text :class="price[index][index2] == item.priceDown ? 'priceYes' : 'price'" @click="priceStatus(index2, item.priceDown, index)">
										{{ item.priceDown }}元
									</text>
									<text :class="price[index][index2] == item.priceUp ? 'priceYes' : 'price'" @click="priceStatus(index2, item.priceUp, index)">
										{{ item.priceUp }}元
									</text>
								</uni-col>
								<uni-col :span="4">
									<view class="dot" style="text-align: center;" @click="orderNum(item, index2, price[index][index2])">
										<uni-icons type="plusempty" size="23" color="#fff"></uni-icons>
									</view>
								</uni-col>
							</uni-row>
						</view>
					</view>
				</scroll-view>
			</uni-col>
		</uni-row>
		<uni-popup ref="popup" type="bottom" backgroundColor="#fff">
			<view class="waper">
				<view style="background-color: rgba(241, 162, 147, 0.2);">
					<uni-row>
						<uni-col :span="12"><p class="commodity">商品</p></uni-col>
						<uni-col :span="3"><p class="commodity">单价</p></uni-col>
						<uni-col :span="5"><p class="commodity">数量</p></uni-col>
						<uni-col :span="4"><p class="commodity">总价</p></uni-col>
					</uni-row>
				</view>

				<scroll-view scroll-y="true" style="height:300px ;">
					<uni-row :gutter="20" v-for="(item, index) in electedOrderList" :key="index">
						<view style="line-height: 30px;color: #333;">
							<uni-col :span="12">
								<view style="text-align: center;">
									<!-- <image :src='item.img'  class="orderInfo"></image> -->
									<!-- <p class='commodity'>商品</p> -->
									{{ item.name }}
								</view>
							</uni-col>
							<uni-col :span="3">
								<view style="text-align: center;">
									<!-- <p class='commodity'>价格</p> -->
									{{ item.electedPrice }}元
								</view>
							</uni-col>
							<uni-col :span="5">
								<view style="text-align: center;display: flex;justify-content: space-around;">
									<view class="dotDown" style="text-align: center;" @click="quantityAddition(index, item, item.electedPrice)">
										<uni-icons type="plus" size="20" color="#fff"></uni-icons>
									</view>
									<!-- <p class='commodity'>数量</p> -->
									{{ item.num }}
									<view class="dotDown" style="text-align: center;" @click="QuantityReduction(index, item, item.electedPrice)">
										<uni-icons type="minus" size="20" color="#fff"></uni-icons>
									</view>
									<!-- 	<view class="dot" style="text-align: center;" >
										<uni-icons type="plusempty" size="23" color="#fff"></uni-icons>
									</view> -->
									<!-- <uni-number-box v-model="item.num" class="nuiNumber"  style="transform:scale(0.8,0.8)" /> -->
								</view>
							</uni-col>
							<uni-col :span="4">
								<view style="text-align: center;">
									<!-- <p class='commodity'>数量</p> -->
									{{ item.totalPrice }}元
								</view>
							</uni-col>
						</view>
					</uni-row>
				</scroll-view>
			</view>
		</uni-popup>
		<view class="bottom2">
			<view class="meOrder" @click="open">
				<uni-badge size="small" :text="orderNumber" absolute="rightTop" type="error" :offset="[0, 10]">
					<view><uni-icons class="name" type="cart-filled" size="30" color="red"></uni-icons></view>

					<text>我的订单</text>
				</uni-badge>
			</view>
			<view class="orderPrice">
				<text>¥{{ sumOfPrices }}</text>
			</view>

			<view class="trueOrder">确认订单</view>
		</view>
		<uni-popup ref="popupMessage" type="message"><uni-popup-message type="success" message="商品添加成功" :duration="1000"></uni-popup-message></uni-popup>
		<uni-popup ref="popupMessageError" type="message"><uni-popup-message type="error" message="请选择价格" :duration="1000"></uni-popup-message></uni-popup>
	</view>
</template>

<script>
export default {
	data() {
		return {
			orderNumber: 0,
			navName: [
				{
					name: '小炒类',
					id: 1,
					list: [
						{
							name: '1',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
						{
							name: '1',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
						{
							name: '1',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
						{
							name: '1',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
					]
				},
				{
					name: '快餐类',
					id: 2,
					list: [
						{
							name: '4',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
						{
							name: '1',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
						{
							name: '1',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
						{
							name: '1',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
						{
							name: '1',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
					]
				},
				{
					name: '汤粉类',
					id: 3,
					list: [
						{
							name: '7',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
						{
							name: '1',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
						{
							name: '1',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
						{
							name: '1',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
					]
				},
				{
					name: '凉拌类',
					id: 4,
					list: [
						{
							name: '11',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
						{
							name: '1',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
						{
							name: '1',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
						{
							name: '1',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
					]
				},
				{
					name: '饮料类',
					id: 5,
					list: [
						{
							name: '14',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
						{
							name: '1',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
						{
							name: '1',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
						{
							name: '1',
							img: '../../static/niunan.jpg',
							num: 1,
							priceDown: '15',
							priceUp: '20'
						},
					]
				}
			],
			idName: '1',
			// price: [],
			priceIndex: '1',
			electedOrderNum: [],
			electedOrderList: [],
			strArr: '',
			strIndex: 0,
			sumOfPrices: 0,
			pH: 0, //窗口高度
			navHeight: 0, //元素的所需高度
			price: [[], [], [], [], []],
			clickId: '',

			topList: [],
			isLeftClick: false
		};
	},
	mounted() {
		let that = this;
		uni.getSystemInfo({
			//调用uni-app接口获取屏幕高度
			success(res) {
				//成功回调函数
				//console.log(res);
				that._data.pH = res.windowHeight; //windoHeight为窗口高度，主要使用的是这个
				let titleH = uni.createSelectorQuery().select('.sv'); //想要获取高度的元素名（class/id）
				titleH
					.boundingClientRect(data => {
						let pH = that._data.pH;

						that._data.navHeight = pH - data.top - 60; //计算高度：元素高度=窗口高度-元素距离顶部的距离（data.top）
					})
					.exec();
			}
		});
		this.getNodesInfo();
	},
	methods: {
		getNodesInfo() {
			//小程序没有doucument和window对象(undefined)
			const query = uni.createSelectorQuery().selectAll('.nameTitle');
			console.log(query, 33);
			query.boundingClientRect().exec(res => {
				let nodes = res[0];
				let arr = [];
				console.log(res, 'llllll');
				nodes.map(item => {
					arr.push(item.top);
				});
				this.topList = arr;
				console.log(this.topList, 666);
			});
		},
		scroll(e) {
			if (this.isLeftClick) {
				this.isLeftClick = false;
				return;
			}
			let scrollTop = e.target.scrollTop+200;
			console.log(scrollTop, 555);
			// 只能变前第三个，最后一个到不了底部，只能用滚动到底部事件
			for (let i = 0; i < this.topList.length; i++) {
				let h1 = this.topList[i];
				let h2 = this.topList[i + 1];
				if (scrollTop > h1 && scrollTop < h2) {
					// console.log(h1,h2,scrollTop)
					this.idName = i + 1;
					// console.log(i,93933)
				}
			}
		},
		// 滚动到底部
		scrolltolower() {
			setTimeout(() => {
				this.idName = 5;
			}, 80);
		},

		//价格高亮按钮
		priceStatus(index, price, elIndex) {
			this.priceIndex = index;
			// this.price[index] = price;
			this.price[elIndex][index] = price;
			this.$forceUpdate();
		},
		//商品数量按钮
		orderNum(item, index, price) {
			if (price == null || price == undefined) {
				return this.$refs.popupMessageError.open();
			}
			// //console.log(this.electedOrderNum,44)
			this.orderNumber++;
			this.strArr = '';

			item.electedPrice = price;

			this.strArr += JSON.stringify(item);

			this.electedOrderNum.push(JSON.parse(this.strArr));

			//console.log(this.electedOrderNum, 'wpos');
			var hash = [];
			for (var i = 0; i < this.electedOrderNum.length; i++) {
				for (var j = i + 1; j < this.electedOrderNum.length; j++) {
					if (this.electedOrderNum[i].name === this.electedOrderNum[j].name && this.electedOrderNum[i].electedPrice === this.electedOrderNum[j].electedPrice) {
						++i;
						j = i;
					}
				}
				this.electedOrderNum[i].num = 0;
				hash.push(this.electedOrderNum[i]);
			}

			//console.log(hash, 45646);
			// 第二步，统计重复个数
			let arr = [];
			hash.forEach(item => {
				this.electedOrderNum.forEach(dd => {
					if (item.name === dd.name && item.electedPrice === dd.electedPrice) {
						item.num++;
						item.totalPrice = item.electedPrice * 1 * item.num;
					}
				});

				arr.push(item.electedPrice * 1);
			});
			//console.log(price, 789);

			// hash.forEach(i => {

			this.sumOfPrices += price * 1;
			// })

			this.electedOrderList = hash;
			this.$refs.popupMessage.open();
		},
		//我的订单数量添加
		quantityAddition(index, item, pirce) {
			this.orderNumber++;
			this.electedOrderList[index].num++;
			//数量添加
			this.electedOrderNum.push(item);
			this.sumOfPrices += pirce * 1;
			item.totalPrice = item.num * (item.electedPrice * 1);
		},
		//我的订单-数量减少
		QuantityReduction(index, item, pirce) {
			if (item.num > 0) {
				item.num--;
				this.orderNumber--;
				this.sumOfPrices -= pirce * 1;
				item.totalPrice = item.num * (item.electedPrice * 1);
			}

			//数量为0时 删除数据
			if (item.num == 0) {
				for (let j = 0; j < this.electedOrderList.length; j++) {
					if (this.electedOrderList[j].num == 0) {
						delete this.electedOrderList[j];
					}
				}
			}

			//删掉的item,清除数组里的数量
			for (let i = 0; i < this.electedOrderNum.length; i++) {
				//判断相同的数据,删掉一条
				if (this.electedOrderNum[i].name == item.name && this.electedOrderNum[i].electedPrice == item.electedPrice) {
					delete this.electedOrderNum[i];
					break;
				}
			}
			//清除删除后面数组里得empty
			this.electedOrderNum = this.electedOrderNum.filter(d => d);
			this.electedOrderList = this.electedOrderList.filter(d => d);
			//console.log('*************', this.electedOrderNum, this.electedOrderList);
			// this.electedOrderList[index].num--;
		},
		// 订单信息弹窗
		open() {
			// 通过组件定义的ref调用uni-popup方法 ,如果传入参数 ，type 属性将失效 ，仅支持 ['top','left','bottom','right','center']
			this.$refs.popup.open(['top']);
		},
		orderClick(id) {
			this.idName = id;
			this.clickId = 'po' + (id-1);

			this.isLeftClick = true;
		}
	}
};
</script>

<style lang="less">
body {
	margin: 0;
	padding: 0;
	/* background-color: #ffffed; */
}

.container {
	font-size: 14px;
}

.commodity {
	font-size: 18px;
	font-weight: 900;
	text-align: center;
}

.imgWapper {
	width: 100%;
	height: 130px;
}

.orderPrice {
	width: 60px;
	height: 60px;
	border-radius: 50%;
	background-image: linear-gradient(to right, #ff0101, #ffaa7f);
	position: absolute;
	left: 50%;
	margin-left: -30px;
	line-height: 60px;
	color: #fff;
}

.price {
	margin-top: 14px;
	width: 50px;
	height: 20px;
	text-align: center;
	color: #a7a6a4;
	border: 1px solid #eeece7;
	/* background-color: #eeece7; */
	background-color: rgba(241, 228, 224, 0.2);
	display: inline-block;
	margin-right: 5px;
}

.priceYes {
	margin-top: 14px;
	width: 50px;
	height: 20px;
	text-align: center;
	color: red;

	display: inline-block;
	margin-right: 5px;
	border: 1px solid red;
	background-color: rgba(241, 162, 147, 0.2);
}

.orderInfo {
	width: 60px;
	height: 60px;
}

.dot {
	width: 30px;
	height: 30px;
	line-height: 30px;
	border-radius: 50%;
	transform: translateY(50%);
	/* font-size: 20px;
		font-weight: 900;
		color: #fff; */
	background-color: #ffaa00;
}

.dotDown {
	margin-top: -5px;
	width: 20px;
	height: 20px;
	line-height: 20px;
	border-radius: 50%;
	transform: translateY(50%);
	/* font-size: 20px;
		font-weight: 900;
		color: #fff; */
	background-color: #ffaa00;
}

.wapper {
	border-bottom: 1px solid #dedede;
	margin-bottom: 5px;
	padding: 5px;
}

.title {
	font-size: 18px;
	font-weight: 900;
}

.dark {
	height: 50px;
	/* border: 1px solid #999; */
	background-color: #e7e7e7;
	text-align: center;
	line-height: 50px;
	font-weight: 700;
}

.darkHighLight {
	background-color: #fff;
	height: 50px;
	/* border: 1px solid #999; */

	text-align: center;
	line-height: 50px;
	font-weight: 700;
}

.waper {
	height: 300px;
}

.bottom2 {
	position: absolute;
	bottom: 0;
	/* background-color: #999; */
	text-align: center;
	height: 60px;
	width: 100%;
	font-size: 20px;
	box-shadow: 0px 2px 6px #000;
	color: #333;
	font-weight: 900;
	/* padding-bottom: 10px; */
	display: flex;
	justify-content: space-between;
}

.trueOrder {
	background-color: red;
	width: 50%;
	line-height: 60px;
	color: #fff;
}

.meOrder {
	background-color: #fff;
	// background-color:rgba(255,170,127,0.);
	width: 50%;
}
</style>
