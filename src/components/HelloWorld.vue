<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>Essential Links</h2>
    <!-- <webpage-shot :visible.sync='visible'></webpage-shot> -->
    <button @click="startShot">start webpage shot</button>
    <div id="container" v-show='visible'>
      <div class="tool">
        <div class="close" @click="close">×</div>
        <div class="ok" @click="ok">√</div>
      </div>
      <canvas id='can' @mousedown="startDraw" @mouseup="endDraw" :width="canW" :height="canH" ></canvas>
      <canvas id='clip' hidden :width="areaWidth" :height="areaHeight" ></canvas>
      <img v-if="clipSrc" hidden :src="clipSrc" crossOrigin='anonymous' alt="">
    </div>
  </div>
</template>

<script>
import webpageShot from "@/components/shot";
import html2canvas from 'html2canvas'
export default {
  name: 'HelloWorld',
  components:{
    webpageShot
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      visible: false,
      clickStart:{
        x:0,y:0
      },
      clickEnd:{
        x:0,y:0
      },
      areaWidth:0,
      areaHeight:0,
      canW: 800,
      canH: 200,
      ctx: null,
      clipSrc: null
    }
  },
  methods:{
    startShot(){
      // 截屏
      let w = document.body.clientWidth //* 0.8
      let h = document.body.clientHeight //* 0.8
      this.ctx = document.getElementById('can').getContext('2d')
      document.getElementById('can').width = document.getElementById('can').width
      this.ctx.globalCompositeOperation = "source-over";
      this.canW = w
      this.canH = h
      this.$nextTick(()=>{
        html2canvas(document.body, {
          canvas: document.getElementById('can'),
          width: w,
          height: h,
          // scale: 0.8
        })
        .then(canvas => {
            this.visible = true
            canvas.id = 'can'
            // canvas.hidden = true
            // canvas.width = w
            // canvas.height = h
            console.log(w, h);
            
        })
      })
    },
    startDraw(e){
      this.clickStart.x = e.offsetX
      this.clickStart.y = e.offsetY
      console.log(e.offsetX,e.offsetY);
    },
    endDraw(e){
      this.clickEnd.x = e.offsetX
      this.clickEnd.y = e.offsetY
      let deltaX = this.clickEnd.x - this.clickStart.x
      let deltaY = this.clickEnd.y - this.clickStart.y
      this.areaWidth = deltaX
      this.areaHeight = deltaY
      
      if(deltaX<50 && deltaY<50){
        return 
      }
      this.ctx.globalCompositeOperation = "destination-atop";
      this.ctx.fillStyle='rgb(26,115,232)'//"#FF0000";
      this.ctx.fillRect(
        this.clickStart.x,
        this.clickStart.y,
        deltaX,
        deltaY);
    },
    close(){
      this.visible = false
    },
    ok(){
      var imgData = this.ctx.getImageData(
        this.clickStart.x,
        this.clickStart.y,
        this.areaWidth,
        this.areaHeight
      );
      console.log(imgData);
      // console.log(imgData.toDataURL("image/jpeg"));
      let clipCan = document.getElementById('clip')
      let ctx = clipCan.getContext('2d')
      ctx.putImageData(imgData,0,0)
      this.visible = false

      // 转成图片标签
      // let dataUrl = clipCan.toDataURL()
      // console.log(dataUrl);
      // this.clipSrc = dataUrl

      // 直接转成blob对象
      clipCan.toBlob(function(blob) {
        console.log(blob);
        
        // blob转img，展示到页面上
        // let newImg = document.createElement("img")
        // let url = URL.createObjectURL(blob);
        // newImg.onload = function() {
        //   URL.revokeObjectURL(url);
        // };
        // newImg.src = url;
        // document.body.appendChild(newImg);
      });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='scss' scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
#container{
    position: fixed;
    z-index: 9999;
    width: 100vw;
    height: 100vh;
    top: 0;
    left: 0;
    // box-shadow: 0px 0px 10px 8px rgb(163, 163, 163) inset;
    background-color: rgba($color: #000000, $alpha: .7);
    .tool{
      position: absolute;
      width: 140px;
      height: 60px;
      top: 0;
      left: 50vw;
      transform: translateX(-50%);
      display: flex;
      justify-content: space-between;
      align-items: center;
      z-index: 9999999999;
      >div{
        width: 60px;
        height: 60px;
        line-height: 60px;
        color: #fff;
        border-radius: 50%;
        font-size: 50px;
        text-align:center;
      }
      .close{
        background-color: rgb(190, 17, 17);
      }
      .close:hover{
        cursor: pointer;
        background: rgb(124, 7, 7);
      }
      .ok{
        background-color: rgb(72, 172, 6);
      }
      .ok:hover{
        cursor: pointer;
        background: rgb(56, 131, 6);
      }
      
    }
    #can{
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%,-50%);
      background-color: rgba(0, 0, 0, .2);
      // width: 80%;
      // height: 80%;
      outline: none;
      border: none;
    }
  }
</style>
