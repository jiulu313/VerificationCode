<template>
  <div class="sb_root">
    <div class="sb_tip_container">
      <span class="sb_tip_text">请完成安全验证</span>
      <img v-on:click="onCloseClicked" class="sb_tip_close" src="../assets/sb_close_normal.png">
    </div>

    <div style="width: 100%;height: 1px;background-color: #e4e7eb;"></div>

    <div style="flex:1;width: 290px;margin: 15px;">

      <canvas ref="canvasRef" width="290" height="175" style="margin: 0 auto;"></canvas>

    </div>

    <div class="sb_sliding_container">
      <div ref="dragBtnMoved" class="sb_moved"></div>
      <span ref="dragBtnRef" class="sb_slider_tips_text">向右拖动滑块填充拼图</span>
      <span class="sb_slide_block">-></span>
    </div>

  </div>

</template>

<script>
  export default {
    name: "SlidingBlock",

    data() {
      return {
        image: null,
        ctx: null,
        ctx2: null,

        //方块的宽高
        cw: 40,
        ch: 40,

        rightX: 230,
        rightY: 100,

        leftX: 20,
        leftY: 100,


        canvasWidth: 290,
        canvasHeight: 175,

        canvasShow: false,
      }
    },

    methods: {
      onCloseClicked() {
        this.ctx.clearRect( this.leftX, this.leftY, this.cw, this.ch)
        this.leftX += 10
        this.ctx.drawImage(this.image, this.rightX, this.rightY, this.cw, this.ch, this.leftX, this.leftY, this.cw, this.ch)

      },

      init() {
        this.canvasRef = this.$refs['canvasRef']
        this.ctx = this.canvasRef.getContext('2d')


        this.generateJigsaw()
      },

      generateJigsaw() {

        let that = this
        this.image = new Image()
        this.image.onload = function () {
          that.ctx.drawImage(that.image, 0, 0, that.canvasWidth, that.canvasHeight)
          that.ctx.drawImage(that.image, that.rightX, that.rightY, that.cw, that.ch, that.leftX, that.leftY, that.cw, that.ch)

          that.ctx.clearRect(that.rightX, that.rightY, that.cw, that.ch)
        }
        this.image.src = 'http://pic31.nipic.com/20130711/8952533_164845225000_2.jpg'


        //右边方块
        // //右方拼图块
        // ctx_yan.save();
        // ctx_yan.lineWidth = 1;
        // ctx_yan.strokeStyle = '#ffffff';
        // ctx_yan.beginPath();
        // ctx_yan.moveTo(cX, cY);
        // ctx_yan.lineTo(cX + s / 2 - r, cY);
        // n = strockArc(ctx_yan, cX);
        // ctx_yan.closePath();
        // ctx_yan.stroke();
        // ctx_yan.clip();
        // imgData = ctx_yan.getImageData(cX, cY - r, s + r + 1, s + 2 * r + 1);
        // ctx_yan.fillStyle = 'rgba(255,255,255,0.5)';
        // ctx_yan.fillRect(0, 0, qX, qY);
        //
        // ctx_yan.restore();
        // ctx_yan.fillStyle = 'rgba(255,255,255,0.0)';
        // ctx_yan.fillRect(0, 0, qX, qY);
      }
    },

    mounted() {
      this.init()
    }

  }
</script>

<style scoped>
  .sb_root {
    display: flex;
    flex-direction: column;
    width: 320px;
    height: 280px;
    background-color: gray;
  }

  .sb_tip_container {
    display: flex;
    width: 100%;
    height: 50px;
    justify-content: space-between;
    align-items: center;
  }

  .sb_tip_text {
    margin-left: 15px;
  }

  .sb_tip_close {
    margin-right: 15px;
  }

  .sb_canvas_container {
    display: none;
    position: relative;
  }

  .sb_sliding_container {
    position: static;
    height: 40px;
    margin-left: 15px;
    margin-right: 15px;
    margin-bottom: 15px;
    box-sizing: border-box;
    background-color: #f7f9fa;
    border-style: solid;
    border-width: 1px;
    border-color: #e4e7eb;
    transform: translate(0, 0)
  }

  .sb_moved {
    position: absolute;
    height: 100%;
    background-color: #7ac23c;
    border-radius: 4px;
    color: #ffffff;
  }

  .sb_slider_tips_text {
    color: #45494c;
    font-size: 14px;
    line-height: 40px;
    text-align: center;
  }

  .sb_slide_block {
    position: absolute;
    left: 0;
    top: 0;
    cursor: pointer;
    transition: background .2s linear;

    display: inline-block;
    color: #000;
    font-weight: bold;
    background-color: #fff;
    height: 36px;
    width: 40px;
    line-height: 36px;
    text-align: center;
    border-style: solid;
    border-width: 1px;
    border-color: #e4e7eb;
  }

  .canvas_cls_hidden {
    margin: 0 auto;
    display: none;
  }

  .canvas_cls_show {
    margin: 0 auto;
    display: block;
  }

</style>
