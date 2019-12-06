<template>
<div id="container" @mousedown="start" @mouseup="end" v-if='visible'>
  <div class="tool" @click="close">×</div>
  <div id='shotArea' v-show="areaShow" class="area" :style="{width:areaWidth+'px',
      height:areaHeight+'px',
      left:areaLeft+'px',
      top:areaTop+'px',}"></div>
  
</div>
</template>
<script>
import html2canvas from 'html2canvas'
export default{
  props:['visible'],
  data(){
    return {
      clickStart:{
        x:0,y:0
      },
      clickEnd:{
        x:0,y:0
      },
      areaWidth:800,
      areaHeight:200,
      areaLeft:0,
      areaTop:0,
      areaShow:false,
    }
  },
  methods:{
    close(){
      this.$emit('update:visible', false)
    },
    start(e){
      this.areaShow = false;
      this.clickStart.x = e.clientX
      this.clickStart.y = e.clientY
      console.log(e.clientX,e.clientY);
      
    },
    end(e){
      this.clickEnd.x = e.clientX
      this.clickEnd.y = e.clientY
      let deltaX = this.clickEnd.x - this.clickStart.x
      let deltaY = this.clickEnd.y - this.clickStart.y
      this.areaWidth = deltaX
      this.areaHeight = deltaY
      
      if(deltaX<50 && deltaY<50){
        return 
      }
      let left = this.clickStart.x
      let top = this.clickStart.y
      this.areaLeft = left
      this.areaTop = top
      this.areaShow = true;
      
    },
  },
  mounted(){
    
  }

}
</script>
<style lang="scss" scoped>
  #container{
    position: fixed;
    z-index: 99999999;
    width: 100vw;
    height: 100vh;
    top: 0;
    left: 0;
    // box-shadow: 0px 0px 10px 8px rgb(163, 163, 163) inset;
    background-color: rgba($color: #000000, $alpha: .7);
    .tool{
      position: absolute;
      width: 60px;
      height: 60px;
      line-height: 60px;
      top: 0;
      left: 48vw;
      text-align:center;
      font-size: 50px;
      color: #fff;
      background: rgb(61, 61, 61);
      border-radius: 50%;
    }
    .tool:hover{
      cursor: pointer;
      background: rgb(41, 41, 41);
    }
    .area{
      position: absolute;
      border: 2px solid #3960e0;
    }
    #can{
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%,-50%);
      background-color: #fff;
    }
  }
</style>