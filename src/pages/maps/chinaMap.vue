<template>
	<div>
		<div ref="mapBox" class="mapbox">
			
		</div>
		<view class="bottombar">
			<router-link to='#' class="btn1"><button>疫情地图</button></router-link>
			<view class="btn2">模块二</view>
			<view class="btn3">模块三</view>
		</view>
	</div>
</template>

<script>
	import echarts from 'echarts'
	import 'echarts/map/js/china'
	import jsonp from 'jsonp'
	const option = {
		title:{
			text:'中国疫情地图',
			link:'https://ncov.dxy.cn/ncovh5/view/pneumonia?from=timeline',
			subtext:'中国疫情地图',
			sublink:'https://ncov.dxy.cn/ncovh5/view/pneumonia?from=timeline',
		},
		series:[{
			name:'确诊人数',
			type:'map',
			map:'china',
			label:{
				show:true,
				color:'#333',
				fontSize:10,
			},
			roam:true,
			zoom:1.2,
			scaleLimit:{                       //所属组件的z分层，z值小的图形会被z值大的图形覆盖
			    min:0.5,                          //最小的缩放值
			    max:3,                            //最大的缩放值
			},
			data:[],//{name:''value:''}
			itemStyle:{
				areaColor:'#eee',
				borderColor:'green',
			},
			//高亮显示
			emphasis:{
				label:{
					color:'white',
					fontSize:12,
				},
				itemStyle:{
					areaColor:'#83b5e7',
				}
			}
		}],
		tooltip:{
			trigger:'item'
		}
	}
	export default{
		name: 'chinaMap',
		mounted(){
			this.getData()
			this.mycharts = echarts.init(this.$refs.mapBox)
			this.mycharts.setOption(option)
		},
		methods:{
			getData() {
				//接口调用（时间戳公布数据）
				jsonp('https://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427'
				,{},(err,data)=>{
					if(!err){
						console.log(data)
						let list = data.data.list.map(item=>{
							return{
								name:item.name,
								value:item.value
							}
						})
						option.series[0].data = list
						
						this.mycharts.setOption(option)
					}
					
				})
			}
		}
	}
</script>

<style>
	.bottombar{
		position: fixed;
		bottom: 0;
		width: 100vw;
		height: 50px;
		background-color: #007AFF;
		display: flex;
		flex-direction:row;
		align-items: center;
		justify-content: space-around;
		.btn1,.btn2,.btn3{
			flex:1;
			
		}
	}
	.mapbox{
		width: 100vw;
		height: 100vh;
	}
</style>
