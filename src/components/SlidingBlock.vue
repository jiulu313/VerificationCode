<template>
  <div class="container">
    <img src="img/bc01.jpg" class="img_cls">
    <div v-bind:class="{canvas_container:!this.canvas_container_cls,canvas_container_block:this.canvas_container_cls}">
      <canvas ref="yan_img" id="yan_img" width="400" height="250"></canvas>
      <canvas ref="yan_ceng" id="yan_ceng" width="400" height="250"></canvas>
    </div>

    <div v-on:mousemove="onMoverMove" class="yan_drag">
      <div ref="mover" class="moved" v-bind:style="{width:this.moverWidth}">{{movedText}}</div>
      <button ref="btn" class="yan_drag_btn" v-on:mousedown.stop="onBtnMouseDown" v-bind:style="{left:this.btnLeft}">
        {{btnText}}
      </button>
      <div class="yan_drag_text">{{dragText}}</div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "SlidingBlock2",

    data() {
      return {
        r: 10,
        s: 50,
        qX: 400,
        qY: 250,
        resultData: null,
        cX: 0,
        cY: 0,
        imgData: null,
        isMatch: false,
        img: null,

        ctx_img: null,
        ctx_ceng: null,

        mX: 0,
        dX: 0,
        dX1: 0,
        isDrag: false,


        //extra
        canvas_container_cls: false,

        btnLeft: '122px',
        moverWidth: 0,
        btnText: '|||',
        movedText : '',
        dragText : '拖动滑块进行验证'

      }
    },

    methods: {
      init() {
        this.ctx_img = this.$refs['yan_img'].getContext('2d')
        this.ctx_ceng = this.$refs['yan_ceng'].getContext('2d')

        this.canvas_container_cls = true

        let that = this
        document.onmouseup = function (e) {
          that.onDocumentMouseUp(e)
        }

        this.generateJigsaw()
      },

      generateJigsaw() {
        this.mX = 0;
        this.btnLeft = 0
        this.moverWidth = 0
        this.ctx_img.clearRect(0, 0, this.qX, this.qY)

        this.img = new Image()

        let that = this
        this.img.onload = function () {
          that.doDraw()
        }

        //右边方块随机位置
        this.cX = 300
        this.cY = 110
        this.img.src = '/static/bc02.jpg'
        this.img.crossOrigin = '';

      },

      doDraw() {
        this.ctx_img.drawImage(this.img, 0, 0, this.qX, this.qY);
        //右方拼图块
        this.ctx_img.save();
        this.ctx_img.lineWidth = 2;
        this.ctx_img.strokeStyle = '#ffffff';
        this.ctx_img.beginPath();
        this.ctx_img.moveTo(this.cX, this.cY);
        this.ctx_img.lineTo(this.cX + this.s / 2 - this.r, this.cY);
        this.strockArc(this.ctx_img, this.cX);
        this.ctx_img.closePath();
        this.ctx_img.stroke();
        this.ctx_img.clip();
        this.imgData = this.ctx_img.getImageData(this.cX, this.cY - this.r, this.s + this.r + 1, this.s + 2 * this.r + 1);
        this.ctx_img.fillStyle = 'rgba(255,255,255,0.5)';
        this.ctx_img.fillRect(0, 0, this.qX, this.qY);

        this.ctx_img.restore();
        this.ctx_img.fillStyle = 'rgba(255,255,255,0.0)';
        this.ctx_img.fillRect(0, 0, this.qX, this.qY);

        //左方拼图块
        this.ctx_ceng.clearRect(0, 0, this.qX, this.qY);
        this.ctx_ceng.lineWidth = 2;
        this.ctx_ceng.strokeStyle = '#ffffff';

        this.ctx_ceng.putImageData(this.imgData, this.mX + 1, this.cY - this.r);
        this.ctx_ceng.globalCompositeOperation = "destination-in";
        this.ctx_ceng.save();
        this.ctx_ceng.beginPath();
        this.ctx_ceng.moveTo(this.mX, this.cY);
        this.ctx_ceng.lineTo(this.mX + this.s / 2 - this.r, this.cY);
        this.strockArc(this.ctx_ceng, this.mX + 1);
        this.ctx_ceng.closePath();
        this.ctx_ceng.fillStyle = 'green';
        this.ctx_ceng.fill();
        this.ctx_ceng.clip();

        this.ctx_ceng.restore();
      },

      //随机生成拼图块
      strockArc(ctx, mX) {
        //上凸下凹
        ctx.arc(mX + this.s / 2, this.cY, this.r, Math.PI, 2 * Math.PI);
        ctx.lineTo(mX + this.s, this.cY);
        ctx.lineTo(mX + this.s, this.cY + this.s);
        ctx.lineTo(mX + this.s / 2 + this.r, this.cY + this.s);
        ctx.arc(mX + this.s / 2, this.cY + this.s, this.r, 2 * Math.PI, Math.PI, true);
        ctx.lineTo(mX, this.cY + this.s);
      },

      onBtnMouseDown(e) {
        if (this.isMatch) {
          return;
        }
        this.dX = e.pageX;
        this.dX1 = e.pageX + this.qX - this.s;

        this.btnText = '<->'
        this.isDrag = true;
      },

      onMoverMove(e) {
        if (this.isDrag) {
          let x = e.pageX;
          if (x >= this.dX && x <= this.dX1) {
            this.mX = e.pageX - this.dX;
          }
          if (x >= this.dX1) {
            this.isDrag = false;
            this.mX = 0;
          }


          this.btnLeft = this.mX + 'px'
          this.moverWidth = this.mX + 'px'


          this.ctx_ceng.clearRect(0, 0, this.qX, this.qY);
          this.ctx_ceng.lineWidth = 2;
          this.ctx_ceng.strokeStyle = '#ffffff';

          this.ctx_ceng.putImageData(this.imgData, this.mX, this.cY - this.r);
          this.ctx_ceng.globalCompositeOperation = "destination-in";

          this.ctx_ceng.save();
          this.ctx_ceng.beginPath();
          this.ctx_ceng.moveTo(this.mX, this.cY);
          this.ctx_ceng.lineTo(this.mX + this.s / 2 - this.r, this.cY);
          this.strockArc(this.ctx_ceng, this.mX);
          this.ctx_ceng.closePath();
          this.ctx_ceng.fillStyle = 'green';
          this.ctx_ceng.fill();
          this.ctx_ceng.clip();

          this.ctx_ceng.restore();
        }
      },

      onDocumentMouseUp(e) {
        this.isDrag = false;
        this.btnText = '|||'

        if (this.mX + 1 <= this.cX + 3 && this.mX + 1 >= this.cX - 3) {
          let flag = true
          if (flag) {
            this.isMatch = true;
            this.movedText = '验证通过'
            this.dragText = ''
          } else {
            this.generateJigsaw()
          }
        } else {
          this.generateJigsaw()
        }
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

  .canvas_container_block {
    display: block;
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
