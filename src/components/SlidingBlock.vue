<template>
  <div class="sb_root">
    <div class="sb_tip_container">
      <span class="sb_tip_text">请完成安全验证</span>
      <img v-on:click="onCloseClicked" class="sb_tip_close" src="../assets/sb_close_normal.png">
    </div>

    <div style="width: 100%;height: 1px;background-color: #e4e7eb;"></div>

    <div style="flex:1;width: 290px;margin: 15px;">
      <div class="sb_canvas_container">
        <canvas ref="sb_img" width="100%" height="100%"></canvas>
        <canvas ref="sb_block" width="100%" height="100%"></canvas>
      </div>
    </div>

    <div class="sb_sliding_container">
      <div ref="dragBtnMoved" class="sb_moved" v-bind:style="{width:dragMoveBtnWidth}"></div>
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
        r: 10,
        s: 40,
        qX: 0,
        qY: 0,
        n: 0,
        resultData: 0,
        cX: 0,
        cY: 0,
        imageData: null,
        isMatch: false,

        mX: 0, //移动距离

        //滑动距离
        dX: 0,
        dX1: 0,

        isDrag: false,

        dragMoveBtnWidth: 0,  //滑过的宽度
        dragBtnLeft: 0,  //滑块的left值


        image: null,
        imageRef: null,
        blockRef: null,
        ctxImage: null,
        ctxBlock: null,
      }
    },

    methods: {
      onCloseClicked() {
        alert('关闭')
      },

      init() {
        this.image = new Image()
        this.imageRef = this.$refs['sb_img']
        this.blockRef = this.$refs['sb_block']

        this.ctxImage = this.imageRef.getContext('2d')
        this.ctxBlock = this.blockRef.getContext('2d')

        //默认滑块位置
        this.cX = 40
        this.cY = 80

        this.qX = this.imageRef.width
        this.qY = this.imageRef.height
        //清除
        this.ctxBlock.clearRect(0, 0, this.qX, this.qY)
        this.ctxImage.clearRect(0, 0, this.qX, this.qY)

        this.generateJigsaw()
      },

      //生成拼图
      generateJigsaw() {
        this.mX = 0
        this.dragMoveBtnWidth = 0
        this.dragBtnLeft = 0

        this.ctxBlock.clearRect(0, 0, this.qX, this.qY)

        this.image.src = 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562834497&di=4b589089a972a3175b4282fd071ccf34&imgtype=jpg&er=1&src=http%3A%2F%2Fimg.pconline.com.cn%2Fimages%2Fphotoblog%2F4%2F5%2F9%2F3%2F4593370%2F200912%2F6%2F1260092648480_mthumb.jpg'

        let that = this
        this.image.onload = () => {
          that.beginDraw()
        }

      },

      beginDraw() {
        //画整个背景
        this.ctxBlock.drawImage(this.image, 0, 0, this.qX, this.qY)

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
    background-color: #fff;
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


</style>
