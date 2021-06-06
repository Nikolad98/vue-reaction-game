<template>
    <div class="box" ref="box" v-show="showBox" @click="handleBoxClick"></div>
</template>

<script>
export default {
    props: ["delay"],
    data() {
        return {
            boxSize: 200,
            PositionY: 0,
            positionX: 0,
            showBox: false,
            responseTime: 0,
            timer: null,
            streak: 0,
            level: 1,
            gameBreak: false,
            gameFinish: false,
            delayCount: this.delay,
        }
    },
    methods: {
        start() {
            this.setPosition()
            this.showBox = true
            this.startTimer()
        },
        setDefault() {
            this.boxSize = 200
            this.streak = 0
            this.level = 1
            this.gameBreak = false
            this.gameFinish = false
        },
        setPosition() {
            this.delayCount = 500 + Math.random() * 2000
            this.$refs.box.style.height = `${this.boxSize}px`
            this.$refs.box.style.width = `${this.boxSize}px`
            this.PositionY = Math.floor(Math.random() * (700 - this.boxSize))
            this.PositionX = Math.floor(Math.random() * (400 - this.boxSize))
            this.$refs.box.style.left = `${this.PositionY}px`
            this.$refs.box.style.top = `${this.PositionX}px`
        },
        startTimer() {
            this.timer = setInterval(() => {
                this.responseTime += 10
            }, 10)
        },
        stopTimer() {
            clearInterval(this.timer)
        },
        restartBox() {
            this.responseTime = 0
            this.streak++
            this.showBox = false
        },
        levelUpRestartBox(){
            this.responseTime = 0
            this.streak = 0
            this.showBox = false
            this.boxSize = this.boxSize / 2
        },
        levelUp() {
            this.level++
            if (this.level === 5) {
                this.stopGame()
            } else {
                this.$emit('levelUpdate', this.level)
                this.levelUpRestartBox()
            }
        },
        stopGame() {
            if (this.level < 5) {
                this.gameBreak = true
            } else{
                this.gameFinish = true
            }
            this.$emit('gameBreak', [this.gameBreak, this.streak, this.level, this.gameFinish])
        },
        handleBoxClick() {
            this.stopTimer()
            if (this.responseTime <= 1000){
                if (this.streak < 4){
                    this.restartBox()
                    setTimeout(() => {
                        this.start()
                    }, this.delayCount)
                } else {
                    this.levelUp()
                    setTimeout(() => {
                        this.start()
                    }, this.delayCount)
                }
            } else {
                this.stopGame()
            }
        }
    },
    mounted() {
        this.setDefault()
        setTimeout(() => {
            this.start()
        },this.delay)
    }
    
}
</script>

<style scoped>

.box{
    height: 0px;
    width: 0px;
    display: flex;
    justify-content: center;
    align-items: center;
    /*background-color: rgb(20, 20, 20);*/
    background-image: url(../assets/clickMe.png);
    background-size: cover;
    background-repeat: no-repeat;
    position: absolute;
    left: calc(700px - 200px);
}

</style>