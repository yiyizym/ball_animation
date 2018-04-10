<template>
  <div class="playground" >
      <Board @addBall="addBall"/>
      <transition v-for="(ball, index) in balls" :key="index"
        appear
        v-on:appear="beforeFall"
        v-on:after-appear="Falling"
        v-bind:css="false"
      >
          <div class="ball" :data-posx="ball.x" :data-posy="ball.y" v-if="!ball.done">
              <div>+</div>
          </div>
      </transition>
  </div>
</template>
<script>
import Board from "./Board.vue";
export default {
  name: "Playground",
  data: function() {
    return {
      balls: []
    };
  },
  components: {
    Board
  },
  methods: {
    addBall(x, y) {
      console.log('addBall', x, y);
      this.balls.push({
          done: false,
          x,
          y
      });
    },
    beforeFall(el, done){
        let t = 2
        el.style.transform = `translate3d(${el.dataset.posx - 10}px, ${-(300 - el.dataset.posy + 10)}px, 0)`
        el.style.transition = `transform ${t}s cubic-bezier(0.4, 0, 1, 1)`
        el.firstChild.style.transform = `translate3d(0px, 0px,0)`
        el.firstChild.style.transition = `transform 2s ease-out`
        setTimeout(() => {
            done()
        }, 16);
    },
    Falling(el){
        el.style.transform = 'translate3d(0, 50px, 0)'
        el.firstChild.style.transform = `translate3d(0, -50px,0)`
    }
  }
};
</script>
<style>
body {
    margin: 0;
}
.plaground {
  margin: 5vh;
  border: 1px solid #999;
}
.ball {
  position: fixed;
  top: 300px;
  left: 0;

}
.ball > div {
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background-color: blue;
    color: #fff;
    text-align: center;
    line-height: 20px;
}
</style>
