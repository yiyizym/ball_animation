<template>
  <div :class="['bin', (notExpanding || allExpaned) ? '' : 'expand']"></div>
</template>
<script>
export default {
    props: {
        collectedBall: {
            type: Number,
            default: 0
        }
    },
    data() {
        return {
            expanded: 0,
            notExpanding: true
        }
    },
    computed: {
        allExpaned(){
            return this.expanded >= this.collectedBall;
        }
    },
    watch: {
        collectedBall() {
            this.triggerExpand()
        }
    },

    methods: {
        triggerExpand(){
            if(this.allExpaned || !this.notExpanding){
                return;
            }
            this.notExpanding = false
            setTimeout(() => {
                this.notExpanding = true
                this.expanded += 1
                this.triggerExpand()
            }, 500)
        }
    }
}
</script>
<style>
.bin {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background-color: brown;
}
.expand {
    animation: expand 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275) infinite;
}

@keyframes expand {
    0%{
        transform: scale(1)
    }

    50%{
        transform: scale(1.2)
    }

    100%{
        transform: scale(1)
    }
}
</style>

