<template lang="pug">
    .container
        audio#clip1
            source(src='https://s3.amazonaws.com/freecodecamp/simonSound1.mp3')
        audio#clip2
            source(src='https://s3.amazonaws.com/freecodecamp/simonSound2.mp3')
        audio#clip3
            source(src='https://s3.amazonaws.com/freecodecamp/simonSound3.mp3')
        audio#clip4
            source(src='https://s3.amazonaws.com/freecodecamp/simonSound4.mp3')

        .simon
            .simon__wrap
                .simon__wrap-block
                    h1(style="text-align:center") Simon Game
                    ul.simon__control
                        li.simon__control-item.blue( @click="startPlayer(0)" :class="{_active: this.options.part === 0}")
                        li.simon__control-item.red( @click="startPlayer(1)" :class="{_active: this.options.part === 1}")
                        li.simon__control-item.green( @click="startPlayer(2)" :class="{_active: this.options.part === 2}")
                        li.simon__control-item.yellow( @click="startPlayer(3)" :class="{_active: this.options.part === 3}")
                        .simon__control-info
                            span(v-if="!this.gameOver") {{options.round}}
                            span(v-else) ПРОИГРЫШ


                .simon__wrap-block
                    .simon__option
                        label.simon__option-item(for="begin") Легкий
                            input(type="radio" name="level" id="begin" value=1.5  v-model.number="game.level" checked )
                        label.simon__option-item(for="middle") Средний
                            input(type="radio" name="level" id="middle" value=1 v-model.number="game.level")
                        label.simon__option-item(for="hard") Сложный
                            input(type="radio" name="level" id="hard" value=0.4 v-model.number="game.level")
                        label.simon__option-item
                    button.simon__btn(@click="btnStart = !btnStart" :class="{_active: this.btnStart}" v-if="!this.gameOver")
                        span(v-if="!btnStart") start
                        span(v-else) Stop
                    button.simon__btn(@click="btnRestart = !btnRestart" :class="{_active: this.btnStart}" v-if="this.gameOver")
                        span restart

                    //div
                        h2 game
                        .simon__data
                            div selection {{this.game.selection}}
                            div round {{this.game.round}}
                            div options.round {{this.options.round}}
                            h2 player
                            div selection {{this.player.selection}}
                            div round {{this.player.round}}
</template>

<script>

    export default {
        name: 'App',
        data: () => ({
            sounds: [
                'https://s3.amazonaws.com/freecodecamp/simonSound1.mp3',
                'https://s3.amazonaws.com/freecodecamp/simonSound2.mp3',
                'https://s3.amazonaws.com/freecodecamp/simonSound3.mp3',
                'https://s3.amazonaws.com/freecodecamp/simonSound4.mp3'
            ],
            btnStart: false,
            btnRestart: false,
            gameOver: false,
            win: false,
            options: {
                play: false,
                part: '',
                round: 1,
            },
            game: {
                round: 0,
                selection: [],
                level: 1.5,
            },
            player: {
                round: 0,
                selection: [],
            }
        }),
        watch: {
            btnStart: function () {
                if (this.btnStart) {
                    this.start()
                }
            },
            btnRestart: function () {
                if (this.btnRestart) {
                    this.restart()
                }
            },
            gameOverEnd: function () {
                if (this.gameOver) {
                    this.game.selection = []
                    this.player.selection = []
                    return false
                }
            }

        },
        methods: {
            start() {
                this.game.selection = []
                this.player.selection = []
                this.playing()
            },
            restart() {
                this.gameOver = false

                this.game.round = 0
                this.player.round = 0
                this.options.round = 1

                this.game.selection = []
                this.player.selection = []
                this.btnRestart = false

                setTimeout(() => {
                    this.playing()
                },  2000 )

            },

            // ХОДИТ ИГРА
            playing() {
                if (!this.btnStart) {
                    return false
                } else {
                    setTimeout(() => {

                        if (this.game.round !== this.options.round && !this.gameOver) {
                            let stepPlay = this.randomStep(0, 3);
                            this.visual(stepPlay)
                            this.game.selection.push(stepPlay)
                            this.game.round++
                            this.playing()

                        } else {
                            this.clearPart()
                        }

                    }, this.game.level * 1000)
                }
            },

            // ХОДИТ ИГРОК
            startPlayer(i) {
                this.visual(i)
                if (this.btnStart) {
                    this.player.selection.push(i)
                    this.checked(i) // ПРОВЕРКА С ХОДОМ ИГРЫ
                }

            },

            // ПРОВЕРКА
            checked() {
                let gameSelected = this.game.selection.slice()
                let playerSelected = this.player.selection.slice()
                let allcheck=0
                for (let i = 0; i < playerSelected.length; i++) {
                    // this.gameOver = gameSelected[i] !== playerSelected[i];
                    if (gameSelected[i] === playerSelected[i]) {
                        this.gameOver = false
                    } else {
                        this.gameOver = true
                    }

                    allcheck++
                }
                if (allcheck === gameSelected.length ) {
                    setTimeout(() => {
                        this.roundClear()
                        this.playing()
                    }, this.game.level * 500)

                }

            },
            roundClear() {
                this.player.round++
                this.options.round++
                this.game.round = 0
                this.game.selection = []
                this.player.selection = []
            },
            randomStep(min, max) {
                return Math.floor(Math.random() * (max - min + 1)) + min;
            },
            clearPart() {
                this.options.part = ''
            },
            visual(i) {
                new Audio(this.sounds[i]).play()
                this.options.part = i
                setTimeout(() => {
                    this.clearPart()
                }, this.game.level * 500)
            }
        },


    }
</script>

<style lang="scss">
    @import "styles/index.scss";
</style>
