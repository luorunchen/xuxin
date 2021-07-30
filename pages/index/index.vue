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


							<text v-for="el,index2 in item.price" :key='index2'
								:class="price==index2&&priceIndex==index?'priceYes':'price'"
								@click="priceStatus(index2,index)">{{el.priceOne}}</text>



						</uni-col>
						<uni-col :span="4">
							<view class="dot" style="text-align: center;" @click="orderNum(item)">
								<uni-icons type="plusempty" size="23" color="#fff"></uni-icons>
							</view>
						</uni-col>
					</uni-row>

				</view>



			</uni-col>
		</uni-row>
		<uni-popup ref="popup" type="bottom" backgroundColor="#fff">
			<view class="waper">
				dasdas
				dasdas
				dasdas
				dasdas
				dasdas
				dasdas
				dasdas
				dasdasdasdasdasdasdasdasdasdasdasdasdasdasdasdasdasdas
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
	</view>

</template>

<script>
	export default {
		data() {
			return {
				productDetails: [{
						name: '麻辣小龙虾',
						img: '../../static/niunan.jpg',
						price: [{
							priceOne: '15元',

						}, {

							priceOne: '20元'
						}, ]
					},
					{
						name: '麻辣小龙虾',
						img: '../../static/niunan.jpg',
						price: [{
							priceOne: '15元',

						}, {

							priceOne: '20元'
						}, ]
					},
				],
				num: 2,
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
				price: "",
				priceIndex: "",
			};
		},
		methods: {
			//价格高亮按钮
			priceStatus(data,index) {
				this.price = data
				this.priceIndex=index
				console.log(data,index)
			},
			//商品数量按钮
			orderNum(item) {
				this.num++
				console.log(item)
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
	}

	.container {

		font-size: 14px;


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
