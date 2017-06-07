<template>
  <div class="star" :class="starType">
    <span v-for="itemClass in itemClasses" :class="itemClass" class="star-item"></span>
  </div>
</template>
<script type="text/javascript">
  var LENGTH = 5;  //定义一些常量
  var CLS_on = 'on';
  var CLS_half = 'half';
  var ClS_off = 'off';
  export default{
      computed:{
          props:{
              size:{
                  type:Number
              },
              score:{
                  type:Number
              }
          },
          starType:function (){
              return 'star-' + this.size;
          },
          itemClasses:function(){    //通过computed计算属性，去获得组件的itemClasses数组，然后循环出每一个星星，并且每一个星星的样式是不同的
              var result = [];  //定义一个需要返回的数组
              var score = Math.floor(this.score*2)/2;
              var hasDecimal = score % 1 !== 0;
              var integer = Math.floor(score);  //亮的星星整数
              for(var i=0;i<integer;i++){
                  result.push(CLS_on);
              }
              if(hasDecimal){
                  result.push(CLS_half);
              }
              while(result.length<LENGTH){
                  result.push(ClS_off)
              }
              return result
          }
      }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  .star
    .star-item
      display inline-block
      background-repeat no-repeat
    &.star-48
      .star-item
        width 20px
        height 20px
        margin-right 22px
        background-size 20px 20px
        &:last-child
          margin-right 0
        &.on
          bg-image('star48_on')
        &.half
          bg-image('star48_half')
        &.off
          bg-image('star48_off')
    &.star-36
      .star-item
        width 15px
        height 15px
        margin-right 17px
        background-size 15px 15px
        &:last-child
          margin-right 0
        &.on
          bg-image('star36_on')
        &.half
          bg-image('star36_half')
        &.off
          bg-image('star36_off')
    &.star-24
      .star-item
        width 10px
        height 10px
        margin-right 3px
        background-size 10px 10px
        &:last-child
          margin-right 0
        &.on
          bg-image('star24_on')
        &.half
          bg-image('star24_half')
        &.off
          bg-image('star24_off')
</style>
