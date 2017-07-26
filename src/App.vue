<template>
  <div id="app">
    <div class='qm'>
      <div class='hb' v-if='autograph==false' @click='toAutograph'>点击此区域编辑签名</div>
      <div class='hb' v-if='autograph==true'>
          <img @click='toDel($event)' src="./assets/del.png" class='del' width="15px" height="15px"/>
          <img @click='toAutograph' src="" ref="target" width="100%" height="100%"/>
      </div>
      <autograph v-if='showAutograph==true' :cancel='cancel' :sure='sure' @picInfo='picInfo'></autograph>
    </div>
  </div>
</template>

<script>
import Autograph from './components/autograph'

export default {
  name: 'app',
  components: {
    Autograph
  },
  data() {
    return {
      autograph: false,
      showAutograph: false,
      ifShow: true
    }
  },
  methods: {
    toAutograph() {
      if(this.ifShow) {
          this.showAutograph = true;
          document.getElementById("app").style.overflow = 'hidden';
      } else {
          this.ifShow = true;
      }
    },
    toDel(e) {
      e.preventDefault();
      this.autograph = false;
      this.ifShow = false;
    },
    cancel() {
      this.showAutograph = false;
      document.getElementById("app").style.overflow = 'auto';
    },
    sure() {
      document.getElementById("app").style.overflow = 'auto';
      this.showAutograph = false;
      this.autograph = true;
    },
    picInfo(Img) {
      setTimeout(function() {
          this.$refs.target.src = Img;
      }.bind(this), 1)
    }
  }
}
</script>

<style>
html, body, #app {
  margin: 0; padding: 0; background-color: #f7f7f7; height: 100%; width: 100%;
}
.qm {
    padding: 10px 10px;
    height: 185px;
    box-sizing: border-box;
    position: relative;
}
.hb {
    background-color: #fff; weight: 100%; height: 100%; font-size: 14px; color: #ccc; padding: 14px 15px;
    padding-left: 20px;
}
.del {
    z-index: 100;position: absolute;margin-top: -5px;margin-left: -15px;
}
</style>
