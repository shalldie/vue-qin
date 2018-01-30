<template>
<div class="vue-qin">
    <span 
        v-for="(item,index) in messages" 
        :key="index" 
        @mouseenter="itemEnter($event,index)" 
        @mousemove="itemMove" 
        @mouseleave="itemLeave"
    >{{item}}</span>
</div>
</template>

<script>
import { Animate } from "./animate";

export default {
    props: {
        content: {  // 内容
            type: String,
            default: ''
        },
        transition: { // 晃动时间
            type: Number,
            default: 500
        },
        recline: {  // 每个字符偏移量
            type: Number,
            default: 0.1
        }
    },
    data() {
        return {
            activeIndex: 0, // 当前操作的item的索引
            pageYBase: 0, // 鼠标在移动item前初始y坐标
            pageYMove: 0, // 鼠标在移动item时候的y坐标
            animations: [] // 所有span的动画
        };
    },
    computed: {
        messages() { // 要显示的文字的数组
            return this.content.split('');
        }
    },
    methods: {
        itemEnter(ex, index) {
            this.activeIndex = index;
            this.pageY = ex.pageY;
        },
        itemMove(ex) {
            this.pageYMove = ex.pageY;
        },
        itemLeave(ex) {

<<<<<<< HEAD
=======
        },
        invokeAnimation() {  // 所有span开始动画
            // 禁止之前的所有动画
            for (let i = 0, len = this.animations.length; i < len; i++) {
                this.animations[i].stop();
            }
>>>>>>> 36f64b97edab6bde9db151e889a5f1be22cdd463
        }
    }
}
</script>

<style scoped>
.vue-qin {
}
</style>
