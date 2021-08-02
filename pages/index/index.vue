<template>
	<view class="container">
		<image src="../../static/niunan.jpg" class="imgWapper"></image>
		<uni-row :gutter='10'>
			<uni-col :span="6">
				<view :class="idName==item.id?'darkHighLight':'dark'" v-for="item,index in navName"
					@click="orderClick(item.id)" :key='index'>{{item.name}}
				</view>
			</uni-col>
			<uni-col :span="18">
				<view class="wapper" v-for="item,index in productDetails" :key='index'>
					<uni-row>
						<uni-col :span="6">
							<view style="text-align: center;">
								<image :src="item.img" class="orderInfo"></image>
							</view>

						</uni-col>
						<uni-col :span="14">
							<view class="title">{{item.name}}</view>

							<!-- 
							<text v-for="el,index2 in item.price" :key='index2'
								:class="price==index2&&priceIndex==index?'priceYes':'price'"
								@click="priceStatus(index2,index)">{{el.priceOne}}</text>
 -->
							<text :class="price[index]==item.priceDown?'priceYes':'price'"
								@click="priceStatus(index,item.priceDown)">
								{{item.priceDown}}
							</text>
							<text :class="price[index]==item.priceUp?'priceYes':'price'"
								@click="priceStatus(index,item.priceUp)">
								{{item.priceUp}}
							</text>


						</uni-col>
						<uni-col :span="4">
							<view class="dot" style="text-align: center;" @click="orderNum(item,index,price[index])">
								<uni-icons type="plusempty" size="23" color="#fff"></uni-icons>
							</view>
						</uni-col>
					</uni-row>

				</view>



			</uni-col>
		</uni-row>
		<uni-popup ref="popup" type="bottom" backgroundColor="#fff">

			<view class="waper">
				<uni-row>
					<uni-col :span="14">
						<p class='commodity'>商品</p>
					</uni-col>
					<uni-col :span="5">
						<p class='commodity'>价格</p>
					</uni-col>
					<uni-col :span="5">
						<p class='commodity'>数量</p>
					</uni-col>
				</uni-row>

				<scroll-view scroll-y="true" style="height:300px ;">
					<uni-row :gutter="20" v-for="item,index in electedOrderList" :key='index'>

						<view style="line-height: 30px;">
							<uni-col :span="14">
								<view style="text-align: center;">
									<!-- <image :src='item.img'  class="orderInfo"></image> -->
									<!-- <p class='commodity'>商品</p> -->
									{{item.name}}
								</view>
							</uni-col>
							<uni-col :span="5">
								<view style="text-align: center;">
									<!-- <p class='commodity'>价格</p> -->
									{{item.electedPrice}}
								</view>
							</uni-col>
							<uni-col :span="5">
								<view style="text-align: center;">
									<!-- <p class='commodity'>数量</p> -->
									{{item.num}}
								</view>
							</uni-col>
						</view>
					</uni-row>


				</scroll-view>



			</view>


		</uni-popup>
		<view class="bottom2">
			<view class="meOrder" @click="open">
				<uni-badge size="small" :text="num" absolute="rightTop" type="error" :offset='[0,10]'>
					<view>
						<uni-icons class="name" type="cart-filled" size="30" color="red"></uni-icons>
					</view>

					<text>我的订单</text>
				</uni-badge>
			</view>

			<view class="trueOrder">
				确认订单
			</view>

		</view>
		<uni-popup ref="popupMessage" type="message">
			<uni-popup-message type="success" message="商品添加成功" :duration="1000"></uni-popup-message>
		</uni-popup>
	</view>

</template>

<script>
	export default {
		data() {
			return {
				productDetails: [{
						name: '麻辣小龙虾',
						img: '../../static/niunan.jpg',
						num: 1,
						priceDown: '15元',
						priceUp: '20元',


					},
					{
						name: '麻辣大龙虾',
						img: '../../static/niunan.jpg',
						num: 1,
						priceDown: '15元',
						priceUp: '20元'

					},
					{
						name: '娃哈哈',
						img: '../../static/niunan.jpg',
						num: 1,
						priceDown: '15元',
						priceUp: '20元'

					},
				],
				num: 0,
				navName: [{
					name: '小炒类',
					id: 1
				}, {
					name: '快餐类',
					id: 2
				}, {
					name: '汤粉类',
					id: 3
				}, {
					name: '凉拌类',
					id: 4
				}, {
					name: '饮料类',
					id: 5
				}],
				idName: "",
				price: [],
				priceIndex: "1",
				electedOrderNum: [],
				electedOrderList:[],
				strArr: '',
				strIndex: 0
			};
		},
		methods: {
			//价格高亮按钮
			priceStatus(index, price) {

				this.priceIndex = index
				this.price[index] = price

				this.$forceUpdate()
				// console.log(index, this.price, 1312)
			},
			//商品数量按钮
			orderNum(item, index, price) {
				// console.log(this.electedOrderNum,44)
				this.num++
				this.strArr = ''
			
				item.electedPrice = price
	
				this.strArr += JSON.stringify(item)
			
				this.electedOrderNum.push(JSON.parse(this.strArr))
			
				console.log(this.electedOrderNum, 217)
				var hash = [];
				for (var i = 0; i < this.electedOrderNum.length; i++) {
					for (var j = i + 1; j < this.electedOrderNum.length; j++) {
						if (this.electedOrderNum[i].name === this.electedOrderNum[j].name && this.electedOrderNum[i]
							.electedPrice === this.electedOrderNum[j].electedPrice) {
							++i;
							j = i;
						}
					}
					this.electedOrderNum[i].num = 0;
					hash.push(this.electedOrderNum[i]);
				}
				// 第二步，统计重复个数
				hash.forEach(item => {
					this.electedOrderNum.forEach(dd => {
						if (item.name === dd.name && item.electedPrice === dd.electedPrice) {
							item.num++
						}
					})
				});
				console.log(hash, 9999)
				this.electedOrderList=hash
				this.$refs.popupMessage.open()
			},
			//订单信息按钮
			open() {
				this.$refs.calendar.open();
			},
			confirm(e) {
				console.log(e);
			},
			// 订单信息弹窗
			open() {
				// 通过组件定义的ref调用uni-popup方法 ,如果传入参数 ，type 属性将失效 ，仅支持 ['top','left','bottom','right','center']
				this.$refs.popup.open(['top'])
			},
			orderClick(id) {
				console.log(11111, id)
				this.idName = id
			}
		}
	}
</script>

<style>
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
		border: 1px solid red;
		display: inline-block;
		margin-right: 5px;
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

	.wapper {
		border-bottom: 1px solid #999999;
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
		background-color: #eeeeee;
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
		width: 50%;

	}
</style>
