<template>
<div class="colorpicker">
  <div class="swatch">
    <a
      v-for="(ps, psidx) in paletteSwatchArr"
      :class="[selColor&&paletteSwatchArr[psidx]['color']==selColor ? 'isCurrent' : '']"
      :style="{background: paletteSwatchArr[psidx]['color']}"
      :data-sid="psidx"
      :key="psidx"
      @click="selectSwatch">
    </a>
  </div>
  <div class="bar">
    <a v-for="(pb, pbidx) in paletteBarArr"
      :style="{background: paletteBarArr[pbidx]['color']}"
      :data-bid="pbidx"
      :key="pbidx"
      @click="switchPaletteSwatch">
    </a>
  </div>
</div>
</template>

<script>
export default {
  name: "GenerColorpicker",
  props: {
    barIdx: Number,
    selColor: String,
  },
  data() {
    return {
      palleteBarIdx: 0,
      paletteBarSelected: 0,
      paletteBarArr: [{color: "#f50f4b"}, {color: "#ef15bc"}, {color: "#ce37ff"}, {color: "#623ae8"}, {color: "#095eef"}, {color: "#2099e5"}, {color: "#11e0c8"}, {color: "#10e051"}, {color: "#d5ef12"}, {color: "#f5da0e"}, {color: "#f5810c"}, {color: "#f4430d"}],
      paletteSwatchArr: [],
    }
  },
  created: function(){
    this.getPaletteSwatch(this.barIdx);
  },
  methods: {
    zeroPad(num, len) {
      return ('0000'+num).slice(-len);
    },
    changeHexRgb(hex) {
      const _hex = hex.slice(1),
        r = parseInt(_hex.slice(0, 2), 16),
        g = parseInt(_hex.slice(2, 4), 16),
        b = parseInt(_hex.slice(4, 6), 16);
      return [r, g, b];
    },
    changeRgbHex(rgb) {
      const _rgb = rgb.indexOf("rgb")<0 ? rgb : rgb.slice(4, -1).split(","),
        __r = Number(_rgb[0]),
        _r = __r>255 ? 255 : __r<0 ? 0 : __r,
        r = this.zeroPad(_r.toString(16), 2),
        __g = Number(_rgb[1]),
        _g = __g>255 ? 255 : __g<0 ? 0 : __g,
        g = this.zeroPad(_g.toString(16), 2),
        __b = Number(_rgb[2]),
        _b = __b>255 ? 255 : __b<0 ? 0 : __b,
        b = this.zeroPad(_b.toString(16), 2);
      return "#"+r+g+b;
    },
    getPaletteSwatch(_idx) {
      this.paletteSwatchArr = [];
      const idx = _idx ? _idx : this.paletteBarSelected,
        base = this.changeHexRgb(this.paletteBarArr[idx].color),
        per = 1/10;
      for(let col=0;col<10;col++) {
        const wh = 255*(10-col)*per,
          r = base[0]*col*per,
          g = base[1]*col*per,
          b = base[2]*col*per;
        for(let row=0;row<10;row++) {
          const bk = 255*row*per,
            bg = this.changeRgbHex([Math.ceil(r+wh-bk), Math.ceil(g+wh-bk), Math.ceil(b+wh-bk)]);
          if(col && row<6) {
            this.paletteSwatchArr.push({color: bg});
          }
        }
      }
    },
    switchPaletteSwatch(e) {
      const btn = e.target,
        idx = Number(btn.dataset.bid);
      this.palleteBarIdx = idx;
      this.getPaletteSwatch(idx);
    },
    selectSwatch(e) {
      const btn = e.target,
        idx = Number(btn.dataset.sid);
      this.paletteBarSelected = this.palleteBarIdx;
      this.$emit("change-swatch", this.paletteSwatchArr[idx].color, this.paletteBarSelected);
    }
  },
}
</script>

<style scoped>
@import "../css/colorpicker.css";
</style>