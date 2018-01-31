<template>
<div class="vue-qin" 
    @mouseenter="wrapEnter" 
    @mousemove="wrapMove"
    @mouseleave="invokeAnimation">
    <span 
        v-for="(item,index) in messages" 
        :key="index" 
        :ref="'item' + index"
        @mouseenter="itemEnter($event,index)"
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
            offsets: [], // 所有span的偏移值 Array<number>
            animations: [] // 所有span的动画 Array<Animate>
        };
    },
    computed: {
        messages() { // 要显示的文字的数组
            return this.content.split('');
        }
    },
    methods: {
        wrapEnter(ex) {
            this.pageYBase = ex.pageY;
        },
        wrapMove(ex) {
            let ifAnimating = this.animations.some(item => item.state === 1);
            if (ifAnimating) {
                return;
            }

            // 偏移量
            let diffY = ex.pageY - this.pageYBase;

            if (diffY == 0) return;

            for (let i = 0, len = this.content.length; i < len; i++) {
                let diff = Math.abs(diffY) - Math.abs(this.activeIndex - i) * this.recline;
                if (diffY < 0) {
                    diff *= -1;
                }
                // 如果距离太远，趋于0
                if (diff * diffY < 0) {
                    diff = 0;
                }

                this.setOffsetStyle(i, diff);
            }

            if (Math.abs(diffY) > this.offset) {
                this.invokeAnimation();
            }
        },
        itemEnter(ex, index) {
            this.activeIndex = index;
        },
        invokeAnimation() {  // 所有span开始动画
            // 禁止之前的所有动画
            // this.animations.forEach(item => item.stop());

            // 如果正在动画，返回
            if (this.animations.some(item => item.state == 1)) return;

            // 重置并开始动画
            this.animations = this.messages.map((k, i) => {
                return new Animate(
                    this.offsets[i] || 0,
                    0,
                    this.duration,
                    num => this.setOffsetStyle(i, num)
                ).start();
            });
        },
        setOffsetStyle(index, num) { // 设置元素偏移
            this.offsets.splice(index, 1, num); // 记录偏移值
            let ele = this.$refs['item' + index][0];

            let styleContent = [
                'msTransform',
                'OTransform',
                'MozTransform',
                'webkitTransform',
                'transform']
                .map(name => `${name}:translate3d(0,${num}px,0);`)
                .join('');

            ele.style.cssText = styleContent;
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
