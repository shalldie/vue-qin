<template>
<div class="vue-qin" @mouseenter="wrapEnter" @mousemove="wrapMove">
    <span 
        v-for="(item,index) in messages" 
        :key="index" 
        @mouseenter="itemEnter($event,index)" 
        :style="{transform:translates[index]}"
    >{{item}}</span>
</div>
</template>

<script>
import Animate from "./animate";

export default {
    props: {
        content: {  // 内容
            type: String,
            default: ''
        },
        duration: { // 晃动时间
            type: Number,
            default: 500
        },
        recline: {  // 每个字符偏移量
            type: Number,
            default: 0.6
        },
        offset: {  // 最大偏移量
            type: Number,
            default: 22
        }
    },
    data() {
        return {
            activeIndex: 0, // 当前操作的item的索引
            pageYBase: 0, // 鼠标在移动item前初始y坐标
            offsets: [], // 每个span的偏移量 Array<number>
            animations: [] // 所有span的动画 Array<Animate>
        };
    },
    computed: {
        messages() { // 要显示的文字的数组
            return this.content.split('');
        },
        translates() {  // 偏移的transform样式
            return this.offsets.map(n => `translate3d(0,${n}px,0)`);
        }
    },
    created() {
        // this.offsets = Array(this.content.length).fill(20);


        // for (let i = 0, len = this.offsets.length; i < len; i++) {
        //     new Animate(this.offsets[i], 0, 1000, num => this.offsets.splice(i, 1, num)).start();
        // }

        // let item = new Animate(0, 100, 3000, num => console.log(num));
        // window.item = item;
        // item.start();
    },
    methods: {
        wrapEnter(ex) {
            this.pageYBase = ex.pageY;
        },
        wrapMove(ex) {
            let arr = Array(this.content.length);

            // 偏移量
            let diffY = ex.pageY - this.pageYBase;

            for (let i = 0, len = arr.length; i < len; i++) {
                let diff = Math.abs(diffY) - Math.abs(this.activeIndex - i) * this.recline;
                if (diffY < 0) {
                    diff *= -1;
                }
                // 如果距离太远，趋于0
                if (diff * diffY < 0) {
                    diff = 0;
                }
                arr[i] = diff;
            }

            this.offsets = arr;

            if (Math.abs(diffY) > this.offset) {
                setTimeout(() => {

                    this.invokeAnimation();
                }, 1000);
            }
        },
        itemEnter(ex, index) {
            this.activeIndex = index;
        },
        invokeAnimation() {  // 所有span开始动画
            // 禁止之前的所有动画
            for (let i = 0, len = this.animations.length; i < len; i++) {
                this.animations[i].stop();
            }

            // 开始动画
            let arr = Array(this.content.length);
            for (let i = 0, len = arr.length; i < len; i++) {
                arr[i] = new Animate(
                    this.offsets[i],
                    0,
                    this.duration,
                    num => this.offsets.splice(i, 1, num),
                    num => this.offsets.splice(i, 1, num)
                ).start();
            }
            this.animations = arr;

        }
    }
}
</script>

<style scoped>
.vue-qin {
    display: inline-block;
}
.vue-qin span {
    display: inline-block;
}
</style>
