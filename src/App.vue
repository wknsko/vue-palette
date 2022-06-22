<template>
  <div class="container">
    <h1>Color Pallette Generator</h1>
    <div class="card">
      <div v-for="(color,i) in 6" :key="color">
        <div class="color" @click="click(i)" :id="i" :style='{ backgroundColor: arrayOfColors[i] }'>
          <div @click.stop.prevent="copyText(arrayOfColors[i])" class="rgb-text" :class="i">{{
            arrayOfColors[i] }}</div>
        </div>

        <div @click.stop.prevent="copyText(arrayOfHexColors[i])">{{ arrayOfHexColors[i] }}</div>
      </div>
    </div>
    <Transition>
      <div v-if="infoShow" id="color-tab">
        <h3>Color Info</h3>
        <div class="info">
          <div><strong>HEX:</strong> {{ RGBtoHex(selectedColor) }}</div>
          <div><strong>RGB:</strong> {{ selectedColor }}</div>
        </div>
        <div class="example">
          <h4>Example</h4>
          <h6 :style="{color: selectedColor}">Title in this color</h6>
          <p :style="{color: selectedColor}">Lorem ipsum dolor sit, amet consectetur adipisicing elit. Quasi, amet.</p>
        </div>
      </div>
    </Transition>
  </div>




</template>

<script>


export default {
  name: 'App',
  data() {
    return {
      array: [0, 1, 2, 3, 4, 5],
      arrayOfColors: [],
      arrayOfHexColors: [],
      selectedColor: '',
      infoShow: false,
      clicked: 420
      
    }
  },
  methods: {
      pickOneColor(){
      return Math.floor(Math.random() * 256)
      },
      pickRGBColor(){
        return `rgb(${this.pickOneColor()}, ${this.pickOneColor()}, ${this.pickOneColor()})`
      },
    RGBtoHex(rgb) {
      rgb = rgb.toString().substring(4, rgb.length).split(")")[0].split(',');
      let r = (+rgb[0]).toString(16);
      let g = (+rgb[1]).toString(16);
      let b = (+rgb[2]).toString(16);
      let hex = `#${r.length === 1 ? r = '0' + r : r}${g.length === 1 ? g = '0' + g : g}${b.length === 1 ? b = '0' + b : b}`;
      return hex;
    },
      getColors(){
        this.array.forEach(el => {
          let rgbColor = this.pickRGBColor();
          this.arrayOfColors.push(rgbColor)
          console.log(el)
          let hex = this.RGBtoHex(rgbColor)
          this.arrayOfHexColors.push(hex)
          
        });
        
      },
      click(idx){
        if(this.clicked != idx) {
        this.selectedColor = this.arrayOfColors[idx];
        this.infoShow = true;
        this.clicked = idx
        } else if (this.clicked == idx) {
          this.infoShow = false
          this.clicked = 420;
          
        } else {
          this.clicked = idx
        }
        
      },
      async copyText(value){
        await navigator.clipboard.writeText(value)
        alert('Copied ' + value )
      }

  },
  mounted() {
    this.getColors();
    
   
  },
  components: {
    
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
  max-width: 700px;
  background-color: #fff;
  width: 100%;
  margin: 0 auto;
}
.card {
  justify-content: center;
  display: flex;
  
}
.card div {
  background-color: #fff;
}
.color {
  width: 80px;
  height:300px;
  margin: 10px;
  border: 2px solid #2c3e50;
  border-radius:10px;
}
.rgb-text {
  width:140px;
  transform: rotate(-90deg);
  margin-top:90px;
  margin-left: -13px;
}
#color-tab {
  width: 400px;
  margin: 0 auto;
}
.example {
  margin-top: 30px;
  border: 2px solid #333;
  border-radius: 20px;
  padding: 20px
}
h3 {
  font-size: 30px;
}
h4 {
  font-size: 20px;
}
h6 {
  font-size: 15px;
}

.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

</style>
