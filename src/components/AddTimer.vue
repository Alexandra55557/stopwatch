<template>
    <div class="add-timer">
        <svg-icon type="mdi" :path="plusPath"></svg-icon>
    </div>
</template>

<script>
import SvgIcon from '@jamescoyle/vue-icon';
import { mdiPlus } from '@mdi/js';

export default {
    name: 'add-timer',

    components: {
        SvgIcon,
    },

    data() {
        return {
            plusPath: mdiPlus,
        }
    }
}
</script>

<style lang="scss">
    .add-timer {
        background: #696969;
        width: 225px;
        height: 120px;
        display: flex;
        justify-content: center;
        align-items: center;
        color: #9E9E9E;
        user-select: none;
        cursor: pointer;
        transition: all .3s;
    }

    .add-timer:hover {
        transition: all .3s;
        color: #fff;
    }
</style>