<template>
  <div class="board" @click="addBall"></div>
</template>
<script>
export default {
    methods: {
        addBall(event){
            console.log('addBall')
            this.$emit('addBall', event.clientX, event.clientY);
        }
    }
}
</script>
<style >
.board {
    box-sizing: border-box;
    width: 300px;
    height: 300px;
    border: 1px solid #333;
}
</style>
