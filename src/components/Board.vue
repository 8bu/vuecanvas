<template>
  <div class="board-wrapper">
    <canvas width="900" height="900" class="board-content" v-insert-message="msg"></canvas>
    <div class="controller">
      <textarea v-model="msg"></textarea>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Board',
  data () {
    return {
      msg: 'Welcome message'
    }
  },
  directives: {
        insertMessage: function(canvasElement, binding) {
            // Get canvas context
            let ctx = canvasElement.getContext("2d");
            let height = canvasElement.height;
            let width = canvasElement.width;
            // Clear the canvas
            ctx.clearRect(0, 0, width, height);
            // Insert stuff into canvas
            ctx.fillStyle = "#04C1F8";
            ctx.fillRect(0,0,width,height);
            ctx.fillStyle = "white";
            ctx.font = "20px Georgia";
            ctx.textAlign = "center";
            ctx.fillText(binding.value, width/2, height/2);
        }
    },
  watch: {
      exampleContent: function() {
          console.log("exampleContent changed to " + this.exampleContent);
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
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
}
</style>
