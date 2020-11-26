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
                        li.simon__control-item.blue( @click="sound(0)" :class="{_active: this.part === 0}")
                        li.simon__control-item.red( @click="sound(1)" :class="{_active: this.part === 1}")
                        li.simon__control-item.green( @click="sound(2)" :class="{_active: this.part === 2}")
                        li.simon__control-item.yellow( @click="sound(3)" :class="{_active: this.part === 3}")
                .simon__wrap-block
                    .simon__option
                        label.simon__option-item Легкий
                            input(type="radio" name="level" value=1.5  :v-bind="this.level" checked)
                        label.simon__option-item Средний
                            input(type="radio" name="level" value=1 :v-bind="this.level")
                        label.simon__option-item Сложный
                            input(type="radio" name="level" value=0.4 :v-bind="this.level")
                        label.simon__option-item
                    //button.simon__btn(@click="this.btn === !this.bnt") Start
                    button.simon__btn(@click="start") Start
</template>

<script>
    import HelloWorld from './components/HelloWorld.vue'

    export default {
        name: 'App',
        data: () => ({
            sounds: [
                'https://s3.amazonaws.com/freecodecamp/simonSound1.mp3',
                'https://s3.amazonaws.com/freecodecamp/simonSound2.mp3',
                'https://s3.amazonaws.com/freecodecamp/simonSound3.mp3',
                'https://s3.amazonaws.com/freecodecamp/simonSound4.mp3'
            ],
            play: false,
            level: 1.5,
            part: '',
            btn: false,
            step: 1
        }),
        components: {
            HelloWorld
        },
        methods: {
            sound(i) {
                new Audio(this.sounds[i]).play()
            },
            start() {
                setTimeout(()=> {
                    this.playing()
                }, this.level * 1000)
            },
            playing() {
                function randomInteger(min, max) {
                    let rand = min + Math.random() * (max + 1 - min);
                    return Math.floor(rand);
                }
                let i = (randomInteger(0, 3))
                console.log(i)
                this.part = i
                new Audio(this.sounds[i]).play()
                this.start()
                // setTimeout(() => this.start , 1000);
            }
        },
        computed: {
            // startPlaying(){
            //     if (this.btn) {
            //         return this.start()
            //     }
            //     return this.start()
            // }
        }

    }
</script>

<style lang="scss">
    @import "styles/index.scss";
</style>
