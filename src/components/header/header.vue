<template>
<div class="header">
	<div class="content-wrapper">
		<div class="avater">
			<img width="64" height="64" :src="seller.avatar">
		</div>
		<div class="content">
			<div class="title">
				<span class="brand"></span>
				<span class="name">{{seller.name}}</span>
			</div>
			<div class="description">{{seller.description}}/{{seller.deliveryTime}}分钟送达
			</div>
			<!-- 这里如果不加v-if会报错，因为在发送ajax之前seller为空对象 -->
			<div v-if="seller.supports" class="support">
				<span class="icon" :class="classMap[seller.supports[1].type]"></span>
				<span class="text">{{seller.supports[1].description}}</span>
			</div>
			<div @click="showDetail" class="count" v-if="seller.supports">{{seller.supports.length}}个<i class="icon-keyboard_arrow_right"></i></div>
		</div>
	</div>
	<div class="bulletin-wrapper"></div>
	<div class="background">
		<img :src="seller.avatar">
	</div>
	<div class="detail" v-show="detailShow">
		<div class="detail-wrapper clearfix">
			<div class="detail-main">
				<h1>{{seller.name}}</h1>
				<v-star :size="48" :score="4.6"></v-star>
			  <div class="title">
				<div class="line"></div>
				<div class="text">优惠信息</div>
				<div class="line"></div>
			  </div>
			  <div class="support">
				<ul v-if="seller.supports">
					<li class="suppor-item" v-for="item in seller.supports">
						<span class="icon decrease" :class="classMap[item.type]"></span>
						<span class="word">{{item.description}}</span>
					</li>
				</ul>
			  </div>
				<div class="title">
					<div class="line"></div>
					<div class="text">商家公告</div>
					<div class="line"></div>
				</div>
				<div class="bulletin">{{seller.bulletin}}</div>
			</div>
		</div>
		<div class="detail-close" @click="hideDetail">
			<i class="icon-close"></i>
		</div>
	</div>
</div>
</template>
<script type="text/javascript">
  import star from '../star/star'
	export default{
    components:{
      'v-star':star
    },
		props:{
			seller:{
				type:Object
			}
		},
		created:function(){
			this.classMap = ['decrease','discount','guarantee','invoice','special']
		},
		data:function(){
			return{
				detailShow:false
			}
		},
		methods:{
			showDetail:function(){
				this.detailShow = true
			},
			hideDetail:function(){
				this.detailShow = false
			}
		}

	}
</script>
<style lang="stylus" rel="stylesheet/stylus">
	 @import "../../common/stylus/mixin.styl"
	 @import "../../common/stylus/base.styl"
	 @import "../../common/fonts/icon.styl"
	 .header
	 	position relative
		 background rgba(7,17,27,.5)   //这里设置i一个蒙层的效果
		.content-wrapper
			display flex        //flex使得两个div能在父容器里面无缝并排
			padding 24px 12px 18px 24px
			color #fff
			font-size 0   //这里可以防止文字与同级元素间的间隔，方法是将父元素fontsize设为0，子元素设置对于字体大小
			.avater
				display inline-block
				vertical-align top
				width 64px
				height 64px
				border-radius 2px
			.content
				position relative
				display inline-block
				width 100%
				font-size 14px
				margin-left 16px
				text-align left
				.title
					margin 2px 0 8px 0
					.brand
						display inline-block
						vertical-align top        //设置inline-block的元素对其，特别是icon和
						width 30px
						height 18px
						bg-image('brand')//每个组件的图在对应的文件夹维护，这里webpack会打包base64
						background-size 30px 18px
						background-repeat no-repeat
					.name
						font-size 16px
						font-weight bold
						line-height 18px
				.description
					font-size 12px
					line-height 12px
					margin-bottom 10px
				.support
					font-size 10px
					line-height 12px
					margin-bottom 2px
					.icon
						display inline-block
						vertical-align top
						width 12px
						height 12px
						background-size 12px 12px
						background-repeat no-repeat
					& .decrease
						bg-image('decrease_1')
					& .discount
						bg-image('discount_1')
					& .guarantee
						bg-image('guarantee_1')
					& .invoice
						bg-image('invoice_1')
					& .special
						bg-image('special_1')
				.count
					position absolute
					right 20px
					bottom 10px
					font-size 10px
					height 24px
					line-height 24px
					background-color rgba(0,0,0,.2)
					padding 0 8px
					border-radius 10px


		.background     //这里设置半透明的蒙层效果
			position absolute
			top 0
			left 0
			width 100%
			height 100%
			z-index -1
			filter blur(10px)  //透视蒙层效果
			img
				width 100%
				height 100%
		.detail
			position fixed
			top 0
			left 0
			width 100%
			height 100%
			z-index 100
			overflow auto
			background rgba(7,17,27,.8)
			color #fff
			.detail-wrapper
				min-height 100%    //最小高度,这里detail是sticky footer的布局套路
				width 100%
				.detail-main
					margin-top 64px
					padding-bottom 64px
					h1
						font-size 16px
						font-weight 700
						line-height 16px
						text-align center
					.star
						text-align center
						margin-top 16px
						margin-bottom 28px
					.title
						display flex
						width 80%
						margin 30px auto 24px auto     //直接可以布局居中
						.line                          //两端横线中间文字的布局
							flex 1
							position relative
							top -6px
							border-bottom 1px solid rgba(225,225,225,0.2)
						.text
							padding 0 12px
							font-weight 700
					.support
						width 80%
						margin 0 auto
						.suppor-item
							font-size 0     //为了抵消图片和文字间的间隔
							line-height 12px
							font-weight 200
							margin-bottom 12px
							&:last-child         //设置最后一个li margin为0
								margin-bottom 0
							.icon
								display inline-block
								vertical-align top
								width 16px
								height 16px
								background-size 16px 16px
								margin-left 12px
								margin-right 6px
							.word
								font-size 12px
								line-height 16px  //这里设置line-height== height 使得span文字居中

							& .decrease
								bg-image('decrease_2')
							& .discount
								bg-image('discount_2')
							& .guarantee
								bg-image('guarantee_2')
							& .invoice
								bg-image('invoice_2')
							& .special
								bg-image('special_2')
					.bulletin
						width 80%
						margin 0 auto
						font-size 12px
						font-weight 200
						line-height 24px

			.detail-close
				position relative
				width 32px
				height 32px
				margin -64px auto 0 auto
				clear both
				font-size 32px
</style>
