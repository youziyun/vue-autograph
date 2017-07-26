<template>
<div>
    <div class='box'>
        <div class='msgbox' ref="msgbox">
            <div class='msgbox-content'>
                <div v-show='qm==false' class='msgbox-zb' @click='toqm'>点击此处后，请在此处签名</div>
                <div v-show='qm==true' calss='msgbox-qm'>
                    <canvas ref="canvas" :width="width" height="175" @touchstart='down($event)' @touchmove='draw($event)' @touchend='up($event)'></canvas>
                </div>
            </div>
            <div class='msgbox-btns'>
                <button @click='toCancel' class='msgbox-btn msgbox-cancel'>取消</button>
                <button @click='exportImg' class='msgbox-btn msgbox-confirm'>确认</button>
            </div>
        </div>
    </div>
    <div class='modal' style='z-index: 2000;'></div>
</div>
</template>

<script>
export default {
    props: {
        cancel: {
            type: Function,
            required: true
        },
        sure: {
            type: Function,
            required: true
        }
    },
    data() {
        return {
            qm: false,
            ifDraw: false,
            width: 0,
            onoff: false,
            oldx: -10,
            oldy: -10,
            lienWidth: 1,
            lineColor: "red",
            clientX: 0,
            clientY: 0
        }
    },
    computed: {
        context: function() {
            const canvaDom = this.$refs.canvas;
            return canvaDom.getContext("2d");
        },
        canva: function() {
            return document.querySelector('canvas');
        }
    },
    mounted() {
        this.width = window.screen.width*0.85;
        this.context.fillStyle = "black";
        this.context.fillRect(0,0,290,175);	
        this.clientX = (window.screen.width*0.15)/2 + 20;
        this.clientY = (window.screen.height-241)/2;
    },
    methods: {
        toqm() {
            this.qm = true;
        },
        down(event){
            event.preventDefault();
            this.onoff = true;
            this.oldx = event.touches[0].clientX - this.clientX;
            this.oldy = event.touches[0].clientY - this.clientY;
        },
        draw(event){
            event.preventDefault();
            this.ifDraw = true;
            if(this.onoff) {
                const newx = event.touches[0].clientX - this.clientX;
                const newy = event.touches[0].clientY - this.clientY;
                this.context.beginPath();
                this.context.moveTo(this.oldx,this.oldy);
                this.context.lineTo(newx,newy);
                this.context.lineWidth = this.lienWidth;
                this.context.lineCap ="round";
                this.context.strokeStyle = 'black';
                this.context.stroke();
                this.oldx = newx;
                this.oldy = newy;
            }
        },
        up(event) {
            event.preventDefault();
            this.onoff = false;
        },
        toCancel() {
            this.cancel();
        },
        exportImg() {
            if(this.ifDraw) {
                this.sure();
                const exportImg = this.canva.toDataURL("image/png");
                this.$emit('picInfo', exportImg);
            } else {
                this.cancel();
            }
            
        }
    }
}
</script>

<style>
.box {
  position: absolute; 
  z-index: 2001;
}

.msgbox {
  position: fixed;
  top: 50%;
  left: 50%;
  -webkit-transform: translate3d(-50%, -50%, 0);
          transform: translate3d(-50%, -50%, 0);
  background-color: #fff;
  width: 85%;
  border-radius: 8px;
  font-size: 16px;
  -webkit-user-select: none;
  overflow: hidden;
  -webkit-backface-visibility: hidden;
          backface-visibility: hidden;
  -webkit-transition: .2s;
  transition: .2s;
}

.msgbox-content {
  padding: 10px 20px 15px;
  border-bottom: 1px solid #ddd;
  min-height: 36px;
  position: relative;
  height:175px;
  padding:0;
  overflow: hidden;
}
.msgbox-zb {
   font-size: 14px; 
   color: #ccc; 
   padding: 10px 20px 15px;
   width: 100%; 
   height: 100%;
 }

 .msgbox-qm {
   width: 100%; 
   height: 100%;
 }

 .msgbox-btns {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  height: 40px;
  line-height: 40px;
}

.msgbox-btn {
  line-height: 35px;
  display: block;
  background-color: #fff;
  -webkit-box-flex: 1;
      -ms-flex: 1;
          flex: 1;
  margin: 0;
  border: 0;
}
.msgbox-btn:focus {
  outline: none;
}
.msgbox-btn:active {
  background-color: #fff;
}

.msgbox-cancel {
  width: 50%;
  border-right: 1px solid #ddd;
}
.msgbox-cancel:active {
  color: #000;
}

.msgbox-confirm {
  color: #26a2ff;
  width: 50%;
}
.msgbox-confirm:active {
  color: #26a2ff;
}

.modal {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  opacity: 0.5;
  background: #000;
}
</style>
