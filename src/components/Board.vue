<template>
  <div class="board-wrapper">
    <canvas width="708" height="591" class="board-content" v-insert-message="format" ></canvas>
    <div class="controller">
      <label for="content">Content</label>
      <textarea id="content" v-model="format.message"></textarea>
      <select v-model="format.size" name="font-size" id="fsize">
        <option value="20px">20px</option>
        <option value="30px">30px</option>
        <option value="40px">40px</option>
        <option value="50px">50px</option>
        <option value="60px">60px</option>
      </select>
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
        size: '20px',
        line_height: 5,
        bg_color: '#04C1F8',
        text_color: '#fff'
      }
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
      ctx.fillRect(0,0,width,height);
      // Loop
      for (let index = 0; index < lines.length; index++) {
        const line = lines[index];
        // Insert stuff into canvas
        ctx.fillStyle = data.text_color;
        ctx.font = data.size + " Georgia";
        ctx.textAlign = "center";
        let lineHeight = parseInt(data.size.split("px")[0]) + 5;
        console.log(lineHeight);
        ctx.fillText(line, width/2, (height/2) + index*lineHeight);
      }
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
  font-size: 15px;
  line-height: 15px;
  color: white;
  background-color: rgba(255,255,255,.1);
  border-radius: 5px;
  border: 0;
  resize: vertical;
}
.controller textarea:focus {
  outline: 0;
}
.controller select {
  background-color: rgba(255,255,255,.1);
  border: 0;
  font-size: 15px;
  text-align: center;
  height: 30px;
  color: white;
  padding: 5px;
  margin: 10px 0;
}
</style>
