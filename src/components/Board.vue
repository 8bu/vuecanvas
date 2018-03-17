<template>
  <div class="board-wrapper">
    <div class="canvas-wrapper">
    <canvas width="708" height="591" class="board-content" v-insert-message="format" >
    </canvas>
    <div class="overlay">Right click outside and choose<br>[Save image as..] or [Copy image]</div>
    </div>
    <div class="controller">
      <label for="content">Content</label>
      <textarea id="content" v-model="format.message"></textarea>
      <label for="font">Font (enter your local font's name)</label>
      <input type="text" v-model="format.font">
      <div class="range">
        <label for="font-size">Size: </label>
        <input v-model="format.size" id="font-size" type="range" min="10" max="200">
      </div>
      <div class="radio" id="radio">
        <label for="radio">Text color: </label>
        
        <input type="radio" name="text1" id="text1" value="#04C1F8" v-model="format.text_color">
        <label for="text1" style="background-color: #04C1F8">Color 1</label>
        <input type="radio" name="text2" id="text2" value="#F03434" v-model="format.text_color">
        <label for="text2" style="background-color: #F03434">Color 2</label>
        <input type="radio" id="text3" value="#8E44AD" v-model="format.text_color">
        <label for="text3" style="background-color: #8E44AD">Color 3</label>
        <input type="radio" id="text4" value="white" v-model="format.text_color">
        <label for="text4" style="background-color: white">Color 4</label>
        <input type="radio" id="text5" value="#00B16A" v-model="format.text_color">
        <label for="text5" style="background-color: #00B16A">Color 5</label>
        <input type="radio" id="text6" value="#F89406" v-model="format.text_color">
        <label for="text6" style="background-color: #F89406">Color 6</label>
        <input type="radio" id="text7" value="#F62459" v-model="format.text_color">
        <label for="text7" style="background-color: #F62459">Color 7</label>
        <input type="radio" id="text8" value="black" v-model="format.text_color">
        <label for="text8" style="background-color: black">Color 8</label>
      </div>
      <div class="radio" id="radio">
        <label for="radio">Background color: </label>
        
        <input type="radio" name="color1" id="color1" value="#04C1F8" v-model="format.bg_color">
        <label for="color1" style="background-color: #04C1F8">Color 1</label>
        <input type="radio" name="color2" id="color2" value="#F03434" v-model="format.bg_color">
        <label for="color2" style="background-color: #F03434">Color 2</label>
        <input type="radio" id="color3" value="#8E44AD" v-model="format.bg_color">
        <label for="color3" style="background-color: #8E44AD">Color 3</label>
        <input type="radio" id="color4" value="white" v-model="format.bg_color">
        <label for="color4" style="background-color: white">Color 4</label>
        <input type="radio" id="color5" value="#00B16A" v-model="format.bg_color">
        <label for="color5" style="background-color: #00B16A">Color 5</label>
        <input type="radio" id="color6" value="#F89406" v-model="format.bg_color">
        <label for="color6" style="background-color: #F89406">Color 6</label>
        <input type="radio" id="color7" value="#F62459" v-model="format.bg_color">
        <label for="color7" style="background-color: #F62459">Color 7</label>
        <input type="radio" id="color8" value="black" v-model="format.bg_color">
        <label for="color8" style="background-color: black">Color 8</label>
      </div>
      <label for="footer_text">Footer</label>
      <input type="text" v-model="format.footer_link">
      <div class="footer-image">
        <label for="footer_text">Footer Image</label>
        <div class="image-wrapper">
          <input type="text" v-model="format.footer_img">
          <!-- <img id="cv_image" :src=resize_img alt="Footer Image"> -->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Board',
  data () {
    return {
      format: {
        message: 'Sample text',
        size: 40,
        line_height: 5,
        bg_color: '#04C1F8',
        text_color: 'white',
        font: "Arial",
        footer_link: 'fb.com/8eebu',
        footer_img: 'https://i.imgur.com/kqrk8r3.png'
      }
    }
  },
  computed: {
    resize_img: async function() {
      return await this.resizeCanvas(this.format.footer_img);
    }
  },
  methods: {
    resizeCanvas: function(src) {
      return new Promise(resolve => {
        let canvas = document.createElement('canvas'),
        ctx = canvas.getContext('2d');
        let img = new Image();
        img.crossOrigin = "Anonymous";
        img.src = src;
        console.log(img.src);
        img.onload = function() {
            // Start resizing
          canvas.height = 100;
          canvas.width = 100;
          ctx.drawImage(this,0,0,this.width, this.height,
                        0,0,canvas.width,canvas.height);
          resolve(canvas);
        };
      })
    }
  },
  directives: {
    insertMessage: function(canvasElement, binding) {
      // Get canvas context
      let ctx = canvasElement.getContext("2d");
      let height = canvasElement.height;
      let width = canvasElement.width;
      let data =  binding.value;
      console.log(binding);
      let lines = data.message.split(/[\n\r]/g);
      console.log(lines);
      // Clear the canvas
      ctx.clearRect(0, 0, width, height);
      ctx.fillStyle = data.bg_color;
      console.log(data.bg_color);
      ctx.fillRect(0,0,width,height);
      // Loop
      for (let index = 0; index < lines.length; index++) {
        const line = lines[index];
        // Insert stuff into canvas
        ctx.fillStyle = data.text_color;
        ctx.font = data.size + "px " + data.font;
        ctx.textAlign = "center";
        let lineHeight = parseInt(data.size) + 5;
        console.log(lineHeight);
        ctx.fillText(line, width/2, (height/2) + index*lineHeight);
      }
      // Insert stuff into canvas
      ctx.fillStyle = data.text_color;
      ctx.font =  "20px " + data.font;
      ctx.textAlign = "center";
      ctx.fillText(data.footer_link, width/2, height - 10);
      let img = document.getElementById("cv_image");
      // ctx.drawImage(img, 0, 0, width , height);
    }
  },
  watch: {
     
  }
}
</script>

<style scoped>
.board-wrapper {
  padding: 50px;
  display: flex;
  align-items: flex-start;
}
.board-content {
  box-shadow: 0 0.5px 0 0 #ffffff inset, 0 1px 2px 0 #B3B3B3;
  background-color: #fff;
  border-radius: 4px;
}
.controller {
  padding: 0 10px;
  display: flex;
  flex: 1;
  flex-direction: column;
}
.controller label {
  text-transform: uppercase;
  color: #fff;
  font-size: 15px;
  font-weight: 600;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif
}
.controller textarea {
  min-height: 100px;
  font-family: Arial, Helvetica, sans-serif;
  padding: 5px;
  margin: 10px 0;
  font-size: 15px;
  line-height: 15px;
  color: white;
  background-color: rgba(255,255,255,.1);
  border-radius: 3px;
  border: 0;
  resize: vertical;
}
.controller input[type="text"] {
  height: 25px;
  font-family: Arial, Helvetica, sans-serif;
  padding: 5px 15px;
  margin: 10px 0;
  font-size: 15px;
  line-height: 15px;
  color: white;
  background-color: rgba(255,255,255,.1);
  border-radius: 3px;
  border: 0;
}
.controller textarea:focus {
  outline: 0;
}

.controller .range {
  display: flex;
  flex-direction: row;
  align-items: center;
}
.controller .range input[type="range"] {
  -webkit-appearance: none;
  -webkit-tap-highlight-color: rgba(255, 255, 255, 0);
  width: 100%;
  height: 10px;
  margin: 0;
  border: none;
  padding: 1px;
  border-radius: 5px;
  background: rgba(255,255,255,.1);
  outline: none; /* no focus outline */
  margin-left: 20px;
}
.controller .range input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  width: 20px;
  height: 20px;
  border: none;
  border-radius: 50%;
  background-image: -webkit-gradient(linear, left top, left bottom, color-stop(0%, #529de1), color-stop(100%, #245e8f)); /* android <= 2.2 */
  background-image: -webkit-linear-gradient(top , #529de1 0, #245e8f 100%); /* older mobile safari and android > 2.2 */;
  background-image: linear-gradient(to bottom, #529de1 0, #245e8f 100%); /* W3C */
}
.controller .footer-image label {
  display: block;
}
.controller .footer-image .image-wrapper {
  display: flex;
  flex-direction: row;
  flex: 1;
}
.controller .footer-image .image-wrapper img {
  max-width: 100px;
  height: 100px;
}
.controller .radio {
  margin-bottom: 20px;
}
.controller .radio label:first-child {
  display: block;
}
.controller .radio input[type="radio"] {
  position: absolute;
  width: 0.1px;
  height: 0.1px;
  top: -100%;
  left: -100%;
}
.controller .radio label:not(:first-child) {
  font-size: 0;
  height: 30px;
  width: 30px;
  display: inline-block;
  transition: all .2s;
}
.controller .radio input[type="radio"]:checked+label{ 
  border-radius: 50%;
  transition: all .2s;
}
.canvas-wrapper {
  position: relative;
  width: fit-content;
}
.canvas-wrapper .overlay{
  position: absolute;
  opacity: 0;
  display: block;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  background-color: rgba(0,0,0,.7);
  color: white;
  font-size: 35px;
  text-align: center;
  padding: 10px;
  width: fit-content;
  word-wrap: break-word;
  max-width: 80%;
  user-select: none;
  transition: all .2s;
}
.canvas-wrapper canvas:hover+.overlay, .canvas-wrapper .overlay:hover {
  opacity: 1;
  transition: all .2s;
}

</style>
