<template>
  <div
    ref="resizer"
    class="resizer"
    :style="`width: ${width}px; height: ${height}px;`"
  >
    <slot></slot>
    <div ref="resizeHandle" class="resizer__handle"></div>
    <div ref="rotateHandle" class="rotate__handle"></div>
    <img src="" />
  </div>
</template>

<script>
export default {
  name: 'Resizer',
  props: {
    width: Number,
    height: Number,
    rotate: Number,
    index: Number,
  },
  data: () => {
    return {
      x: null,
      y: null,
      w: null,
      h: null,
      angle: 0,
    }
  },
  methods: {
    getCenter(element) {
      const {left, top, width, height} = element.getBoundingClientRect();
      return {x: left + width / 2, y: top + height / 2}
    },

    mouseDownResizeHandler(e) {
      e.preventDefault();

      // Get the current mouse position
      this.x = e.clientX;
      this.y = e.clientY;

      // Calculate the dimension of element
      const styles = window.getComputedStyle(this.$refs.resizer);
      this.w = parseInt(styles.width, 10);
      this.h = parseInt(styles.height, 10);

      // Attach the listeners to `document`
      document.addEventListener('mousemove', this.mouseMoveResizeHandler);
      document.addEventListener('mouseup', this.mouseUpResizeHandler);
    },

    mouseMoveResizeHandler(e) {
      // How far the mouse has been moved
      const dx = e.clientX - this.x;
      const dy = e.clientY - this.y;

      // Adjust the dimension of element
      this.$refs.resizer.style.width = `${this.w + dx}px`;
      this.$refs.resizer.style.height = `${this.h + dy}px`;
    },

    mouseUpResizeHandler(e) {
      // Remove the handlers of `mousemove` and `mouseup`
      document.removeEventListener('mousemove', this.mouseMoveResizeHandler);
      document.removeEventListener('mouseup', this.mouseUpResizeHandler);
      
      // How far the mouse has been moved
      const dx = e.clientX - this.x;
      const dy = e.clientY - this.y;

      this.$emit('resize', {
        width: this.w + dx,
        height: this.h + dy,
        index: this.$props.index,
      });
    },

    rotateElement(clientX, clientY) {
      let elementCenter = this.getCenter(this.$refs.resizer);
      this.angle = Math.atan2(clientY - elementCenter.y, clientX - elementCenter.x);
      this.$refs.resizer.style.transform = `rotate(${this.angle}rad)`;
    },

    mouseDownRotateHandler(e) {
      e.preventDefault();
      this.rotateElement(e.clientX, e.clientY);

      // Attach the listeners to `document`
      document.addEventListener('mousemove', this.mouseMoveRotateHandler);
      document.addEventListener('mouseup', this.mouseUpRotateHandler);
    },

    mouseMoveRotateHandler(e) {
      this.rotateElement(e.clientX, e.clientY);
    },

    mouseUpRotateHandler(e) {
      e.preventDefault();
      // Remove the handlers of `mousemove` and `mouseup`
      document.removeEventListener('mousemove', this.mouseMoveRotateHandler);
      document.removeEventListener('mouseup', this.mouseUpRotateHandler);

      // this.$emit('rotate', {
      //   rotate: this.angle,
      //   index: this.$props.index,
      // });
    },
  },

  mounted() {
    this.$refs.resizeHandle.addEventListener('mousedown', this.mouseDownResizeHandler);
    this.$refs.rotateHandle.addEventListener('mousedown', this.mouseDownRotateHandler);
    this.angle = this.$props.rotate;
    this.$refs.resizer.style.transform = `rotate(${this.angle}rad)`;
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

.rotate__handle {
  position: absolute;
  bottom: calc(100% - 5px);
  left: calc(100% - 5px);
  width: 8px;
  height: 8px;
  border: 1px solid rgb(150,150,150);
  border-radius: 50%;
  background-color: rgb(255,255,255);
  cursor: url(~@/assets/cursor-rotate.png) 5 15, auto;
}
</style>
