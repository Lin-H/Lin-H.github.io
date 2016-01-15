title: 内滚动
date: 2015-12-27 13:44:45
categories: Javascript
tag: 
- Javascript
- Vue
keywords: "内滚动,scroll,vue,自定义滚动条"
banner: http://linhsblog-10013469.image.myqcloud.com/images/inner-scroll.png
---

内滚动是一种布局，将传统的网页改造成类似于桌面软件的布局。像现在的酷我音乐盒的主体界面就是一个内滚动布局QQ音乐也是，将浏览器的滚动条用`overflow: hidden`隐藏，再用HTML和CSS画一个滚动条出来，通过JavaScript来处理滚动事件。这么做的好处一是增强用户体验，网页能有客户端的体验；二是可以自定义滚动条。下面我是用Vue做了个内滚动组件，用来简单介绍内滚动的实现。
<!-- more  -->
使用例子(省去了部分标签和代码)
```html
<body>
    <div style="height: 200px;position: relative" class="scroll-view">
        <scroll>
            <div class="scroll-panel">
                <p>1</p>
                <p>2</p>
                <p>3</p>
                <p>4</p>
                <p>5</p>
                <p>6</p>
                <p>7</p>
                <p>8</p>
                <p>9</p>
                <p>10</p>
            </div>
        </scroll>
    </div>
    <script>
        Vue.component('scroll', scroll);
        new Vue({
            el: 'body'
        });
    </script>
</body>
```

`scroll-view`是滚动条的视窗就是能看到的部分，需要设置`position`为`relative`或`absolute`，`scroll-panel`是滚动的内容。使用时将需要滚动的内容放入自定义标签`scroll`内即可

当时写这个内滚动组件的时候参考了`jQuery`的插件`jscrollpanel`。在HTML结构上类似，并做了简化。使用两个div包裹滚动的内容，使用CSS的`top`来实现滚动，再添加两个div用于自定义滚动条

```js
export default {
    template: `
    <div class="vue-scroll">
        <div class="v-scroll-panel" :style="{top: scrollPanelTop + 'px'}">
            <slot></slot>
        </div>
    </div>
    <div class="v-scroll-bar" v-show="isShow">
        <div class="v-scroll-block" @mousedown="dragStart" :style="{top: scrollBlockTop + 'px', height: scrollBlockHeight + 'px'}" :class="{dragging: dragInfo.dragging}" @mousemove="dragging">
        </div>
    </div>
    `,
    data() {
        return {
            scrollBlockTop: 0,//滑块距离顶部高度
            dragInfo: {
                dragging: false,
                Y: 0
            },
            scrollableHeight: 0,//滑动视窗高度
            scrollPanelHeight: 0//滑动区域高度
        };
    },
    computed: {
        scrollBlockHeight() {//滑动块长度
            return this.scrollableHeight * this.scrollableHeight / this.scrollPanelHeight;
        },
        scrollPanelTop() {//实现滚动
            return -~~(this.scrollBlockTop * this.scrollPanelHeight / this.scrollableHeight);
        },
        isShow() {//滚动内容超出视窗范围时显示滚动条
            return this.scrollableHeight < this.scrollPanelHeight;
        }
    },
    ready() {
        this.$el.parentNode.style.overflow = 'hidden';
        this.scrollPanelHeight = this.$el.nextElementSibling.clientHeight;
        this.scrollableHeight = this.$el.parentNode.clientHeight;
        if (this.isShow) {
            this.$el.parentNode.onwheel = (e) => {
                let cal = this.scrollBlockTop + -e.wheelDeltaY / 4 * this.scrollableHeight / this.scrollPanelHeight;
                //限制滚动块滚动范围
                if (cal < 0) {
                    cal = 0;
                } else if (cal >= this.scrollableHeight - this.scrollBlockHeight) {
                    cal = this.scrollableHeight - this.scrollBlockHeight;
                }
                this.scrollBlockTop = cal;
                return false;
            };
        }
    },
    methods: {
        dragStart(e) {
            this.dragInfo.dragging = true;
            this.dragInfo.Y = e.clientY + document.body.scrollTop - this.scrollBlockTop;
            //添加document事件，是为了能使拖动滑块时鼠标离开滚动区域也能拖动滑块
            document.addEventListener('mousemove', this.dragging.bind(this));
            document.addEventListener('mouseup', this.dragEnd.bind(this));
        },
        dragging(e) {
            if (!this.dragInfo.dragging) {
                return;
            }
            let cal = e.clientY + document.body.scrollTop - this.dragInfo.Y;
            if (cal < 0) {
                cal = 0;
            } else if (cal >= this.scrollableHeight - this.scrollBlockHeight) {
                cal = this.scrollableHeight - this.scrollBlockHeight;
            }
            this.scrollBlockTop = cal;
        },
        dragEnd() {
            this.dragInfo.dragging = false;
        }
    },
    events: {
        resize() {
            this.scrollPanelHeight = this.$el.nextElementSibling.clientHeight;
        }
    },
    watch: {
        'dragInfo.dragging': function(val) {
            if (!val) {
                document.removeEventListener('mousemove', this.dragging.bind(this));
                document.removeEventListener('mouseup', this.dragEnd.bind(this));
            }
        }
    }
};
```

首先是计算滑块的长度，其实滚动条可以看成是缩小的浏览器窗口，滑块就是当前浏览器窗口在整个页面中的位置，所以只需要按照比例设置。`scrollPanelHeight / scrollableHeight`等于 `滚动条长度/滑块长度`。在滚动时，滚动的距离也是通过这个比例来决定。在处理鼠标滚动事件时,需要注意浏览器兼容，`IE`,`Firefox`和`Chrome`都不一样。将`scroll-panel`的`top`属性设置为负值，来实现向下滚动的效果。下面是一个用上面代码实现的一个例子,请使用`Chrome`或`webkit`内核浏览器查看

{% raw %}
<div style="height: 200px;position: relative;background-color: #f1f1f1;" class="scroll-view">
    <scroll>
        <div class="scroll-panel" style="background-color: #f1f1f1;">
            <p>1</p>
            <p>2</p>
            <p>3</p>
            <p>4</p>
            <p>5</p>
            <p>6</p>
            <p>7</p>
            <p>8</p>
            <p>9</p>
            <p>10</p>
        </div>
    </scroll>
</div>
<script src="./scroll" type="text/javascript"></script>
{% endraw %}
