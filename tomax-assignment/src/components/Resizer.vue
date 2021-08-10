<template>
  <div
    ref="resizer"
    class="resizer"
    :style="`width: ${width}px; height: ${height}px;`"
  >
    <slot></slot>
    <div ref="handle" class="resizer__handle"></div>
  </div>
</template>

<script>
export default {
  name: 'Resizer',
  props: {
    width: Number,
    height: Number,
    index: Number,
  },
  data: () => {
    return {
      x: null,
      y: null,
      w: null,
      h: null,
    }
  },
  methods: {
    mouseDownHandler(e) {
      e.preventDefault();

      // Get the current mouse position
      this.x = e.clientX;
      this.y = e.clientY;

      // Calculate the dimension of element
      const styles = window.getComputedStyle(this.$refs.resizer);
      this.w = parseInt(styles.width, 10);
      this.h = parseInt(styles.height, 10);

      // Attach the listeners to `document`
      document.addEventListener('mousemove', this.mouseMoveHandler);
      document.addEventListener('mouseup', this.mouseUpHandler);
    },
    mouseMoveHandler(e) {
      // How far the mouse has been moved
      const dx = e.clientX - this.x;
      const dy = e.clientY - this.y;

      // Adjust the dimension of element
      this.$refs.resizer.style.width = `${this.w + dx}px`;
      this.$refs.resizer.style.height = `${this.h + dy}px`;
    },
    mouseUpHandler(e) {
      // Remove the handlers of `mousemove` and `mouseup`
      document.removeEventListener('mousemove', this.mouseMoveHandler);
      document.removeEventListener('mouseup', this.mouseUpHandler);
      
      // How far the mouse has been moved
      const dx = e.clientX - this.x;
      const dy = e.clientY - this.y;

      this.$emit('resize', {
        width: this.w + dx,
        height: this.h + dy,
        index: this.$props.index,
      });
    },
  },
  mounted() {
    this.$refs.handle.addEventListener('mousedown', this.mouseDownHandler);
  }
}
</script>

<style scoped>
.resizer {
  position: relative;
}

.resizer__handle {
  position: absolute;
  top: calc(100% - 5px);
  left: calc(100% - 5px);
  width: 8px;
  height: 8px;
  border: 1px solid rgb(150,150,150);
  background-color: rgb(255,255,255);
  cursor: nwse-resize;
}
</style>
