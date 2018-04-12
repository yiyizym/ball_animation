<template>
  <div class="playground" >
      <Board @addBall="addBall"/>
      <Bin :collectedBall="collectedBall"/>
      <transition v-for="(ball, index) in balls" :key="index"
        appear
        v-on:appear="beforeFall"
        v-on:after-appear="Falling"
        v-bind:css="false"
      >
          <div class="ball" :data-index="index" :data-posx="ball.x" :data-posy="ball.y" v-if="!ball.done">
              <svg>
                  <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#add"></use>
              </svg>
          </div>
      </transition>
      <Icons></Icons>
  </div>
</template>
<script>
import Board from "./Board.vue";
import Bin from "./Bin.vue";
import Icons from "./Icons.vue";
export default {
  name: "Playground",
  data: function() {
    return {
      balls: [],
      collectedBall: 0,
      ballFinalPos: {
          x: 0,
          y: 0
      }
    };
  },
  components: {
    Board,
    Bin,
    Icons
  },
  methods: {
    addBall(x, y) {
      this.balls.push({
          done: false,
          x,
          y
      });
    },
    beforeFall(el, done){
        let t = 2
        el.style.transform = `translate3d(${el.dataset.posx - this.ballFinalPos.x - 10}px, ${-(this.ballFinalPos.y - el.dataset.posy - 10)}px, 0)`
        el.style.transition = `transform ${t}s cubic-bezier(0.4, 0, 1, 1)`
        el.firstChild.style.transform = `translate3d(0px, 0px,0)`
        el.firstChild.style.transition = `transform 2s ease-out`

        el.firstChild.addEventListener('transitionend', ()=>{
            this.balls[el.dataset.index].done = true
            this.collectedBall += 1
        }, {
            passive: true
        })

        setTimeout(() => {
            done()
        }, 16)
    },
    Falling(el){
        el.style.transform = 'translate3d(0, 50px, 0)'
        el.firstChild.style.transform = `translate3d(0, -50px,0)`
    }
  },
  mounted(){
      let pgPos = document.querySelector('.playground').getBoundingClientRect()
      this.ballFinalPos.x = pgPos.left
      this.ballFinalPos.y = pgPos.bottom
  }
};
</script>
<style>
body {
    margin: 0;
}
.playground {
    position: relative;
    box-sizing: border-box;
    width: 300px;
    margin: 50px auto;
    cursor: pointer;
}
.ball {
  position: absolute;
  bottom: 0;
  left: 0;
}
.ball > svg {
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background-color: blue;
    fill: #fff;
    text-align: center;
    line-height: 20px;
}
</style>
