<template>
    <div class="timer">
        <div class="timer__clock" :class="{'timer__clock--play': isPlay}">
            {{ time }}
        </div>
        <div class="timer__delete" @click="$emit('remove', index)">
            <svg-icon type="mdi" :path="closePath"></svg-icon>
        </div>
        <div class="timer__buttons">
            <div class="timer__buttons--play" :class="{'timer__is-play': isPlay}" @click="togglePlay">
                <svg-icon type="mdi" :path="playPath" v-if="!isPlay"></svg-icon>
                <svg-icon type="mdi" :path="pausePath" v-else></svg-icon>
            </div>
            <div class="timer__buttons--reset" :class="{'timer__is-play': isPlay}" @click="reset">
                <svg-icon type="mdi" :path="stopPath"></svg-icon>
            </div>
        </div>
    </div>
</template>

<script>
import { mdiPlay, mdiStop, mdiPause, mdiClose } from '@mdi/js';
import SvgIcon from '@jamescoyle/vue-icon';

export default {
    name: 'timer',

    components: {
        SvgIcon,
    },

    props: {
        timer: {
            type: Number,
            default: 0,
        },

        index: {
            type: Number,
            default: 0,
        }
    },

    data() {
        return {
            playPath: mdiPlay,
            stopPath: mdiStop,
            pausePath: mdiPause,
            closePath: mdiClose,
            time: '00:00',
            intervalID: null,
            isPlay: false,
            seconds: 0,
        }
    },

    methods: {
        togglePlay() {
            this.isPlay = !this.isPlay;
        },

        reset() {
            this.seconds = 0;
            this.time = '00:00';
            this.isPlay = false;
            this.$emit('change', this.seconds, this.index);
        }
    },

    mounted() {
        this.seconds = this.timer;
        this.intervalID = setInterval(() => {
            if (this.isPlay) {
                this.seconds++;
                this.$emit('change', this.seconds, this.index);
            }
            const hours = (this.seconds / 3600) < 10 ? '0' + Math.floor(this.seconds / 3600) : Math.floor(this.seconds / 3600);
            let minutes = (this.seconds / 60) < 10 ? ('0' + Math.floor(this.seconds / 60)) : Math.floor(this.seconds / 60);
            let currentSec = (this.seconds % 60) < 10 ? '0' + this.seconds % 60 : this.seconds % 60;

            if (this.seconds >= 3600) {
                minutes = ((this.seconds % 3600) / 60) < 10 ? '0' + Math.floor((this.seconds % 3600) / 60) : Math.floor((this.seconds % 3600) / 60);
                this.time = hours + ':' + minutes + ':' + currentSec;
            }

            if (this.seconds < 3600) {
                this.time = minutes + ':' + currentSec;
            }

            if (this.seconds < 60) {
                const currentSec = this.seconds < 10 ? '0' + this.seconds : this.seconds;
                this.time = minutes + ':' + currentSec;
            }
        }, 1000);
    },
}
</script>

<style lang="scss">
    .timer {
        background: #696969;
        width: 225px;
        height: 120px;
        // padding: 20px 0;
        display: flex;
        flex-flow: column;
        gap: 15px;
        justify-content: center;
        align-items: center;
        position: relative;

        &__delete {
            position: absolute;
            top: 5px;
            right: 5px;
            color: #9E9E9E;
            cursor: pointer;
        }

        &__delete:hover {
            transition: all .3s;
            color: #fff;
        }

        &__clock {
            font-size: 22px;
            user-select: none;
            border-bottom: 1px solid #9E9E9E;
            width: 100%;
            text-align: center;
            padding-bottom: 20px;
            color: #9E9E9E;
            transition: all .3s;

            &--play {
                transition: all .3s;
                color: #fff;
                border-bottom: 1px solid #fff;
            }
        }

        &__buttons {
            display: flex;
            flex-flow: row nowrap;
            gap: 50px;

             &--play {
                color: #9E9E9E;
                transition: all .3s;
                cursor: pointer;
             }

             &--reset {
                color: #9E9E9E;
                transition: all .3s;
                cursor: pointer;
             }
        }

        &__is-play {
            color: #fff;
            transition: all .3s;
        }
    }
</style>