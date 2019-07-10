<template>
  <div class="container">
    <img src="img/bc01.jpg" class="img_cls">
    <div class="canvas_container">
      <canvas ref="yan_img" id="yan_img" width="400" height="250"></canvas>
      <canvas ref="yan_ceng" id="yan_ceng" width="400" height="250"></canvas>
    </div>

    <div class="yan_drag">
      <div class="moved"></div>
      <button class="yan_drag_btn">|||</button>
      <div class="yan_drag_text">拖动滑块进行验证</div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "SlidingBlock2",

    data() {
      return {
        r : 10,
        s : 50,
        qX : 400,
        qY : 250,
        resultData : null,
        cX : 0,
        cY : 0,
        imgData : null,
        isMatch : false,
        img : null,

        ctx_img : null,
        ctx_ceng : null,

        mX : 0,
        dX : 0,
        dX1 : 0,
        isDrag : false,

        //extra
        canvas_container_cls : false,



      }
    },

    methods: {
      onCloseClicked() {
        this.ctx.clearRect(this.leftX, this.leftY, this.cw, this.ch)
        this.leftX += 10
        this.ctx.drawImage(this.image, this.rightX, this.rightY, this.cw, this.ch, this.leftX, this.leftY, this.cw, this.ch)

      },

      init() {
        this.ctx_img = this.$refs['yan_img'].getContent('2d')
        this.ctx_img = this.$refs['yan_ceng'].getContent('2d')



        this.generateJigsaw()
      },

      //随机生成拼图块
      strockArc(ctx, mX) {
        //上凸下凹
        ctx.arc(mX + s / 2, cY, r, Math.PI, 2 * Math.PI);
        ctx.lineTo(mX + s, cY);
        ctx.lineTo(mX + s, cY + s);
        ctx.lineTo(mX + s / 2 + r, cY + s);
        ctx.arc(mX + s / 2, cY + s, r, 2 * Math.PI, Math.PI, true);
        ctx.lineTo(mX, cY + s);
      }


    },

    mounted() {
      this.init()
    }

  }
</script>

<style scoped>
  .container {
    width: 400px;
    height: 250px;
    margin: 0 auto 0;
    background-color: #7ac23c;
  }

  .img_cls {
    display: none;
  }

  .yan_drag {
    margin-top: 30px;
    border-radius: 4px;
    height: 25px;
    width: 400px;
    box-sizing: border-box;
    line-height: 25px;
    text-align: center;
    background-color: #dddddd;
    position: relative;
  }

  .yan_drag_btn {
    width: 50px;
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
  }

  .canvas_container {
    display: none;
    position: relative;
  }

  #yan_ceng {
    position: absolute;
    top: 0;
    left: 0;
  }

  .exm {
    position: absolute;
    width: 100%;
    height: 25px;
    text-align: center;
    line-height: 25px;
    bottom: 5px;
    left: 0;
    background-color: transparent;
    color: transparent;
    transition: all 0.5s linear;
  }

  .moved {
    position: absolute;
    height: 100%;
    background-color: #7ac23c;
    width: 0;
    color: #ffffff;
  }


</style>
