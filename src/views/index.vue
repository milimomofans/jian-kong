<template>
    <div class="container">
        <div class="c-header-home_header" @mousemove="testHandle">
            <div class="left">
                <div class="now-time">{{  nowTime }} </div>
            </div>
            <div class="org-name-area">
                <div class="org-name-area-main cursor-pointer"  @click="toggleMenu">
                    <div v-if="jumpTitle.length && jumpTitle.length > 5" style="height:.4rem;overflow: hidden;;margin-top: .06rem;width: 80%;">
                        <Vue3SeamlessScroll 
                            direction="left"
                            :hover="true"
                            :limitScrollNum="1"
                            :step="0.5"
                            :isRemUnit="true"
                            :isWatch="true"
                            :list="dataList"
                        >
                            <div class="org-name">
                                {{ jumpTitle }}
                            </div>
                        </Vue3SeamlessScroll>
                    </div>
                    <div v-else class="org-name">{{ jumpTitle.length ? jumpTitle : title }} </div>
                </div>
            </div>
            <div class="right">
                <div class="buttons">
                    <a-tooltip :content="isScreenFull ? '退出全屏' : '按下F11进入全屏'">
                        <img style="height: .2rem;" src="../assets/screenfull.png" alt="screen-full"
                            @click="onScreenFullClick" v-show="!isScreenFull" />
                        <img style="height: .2rem;" src="../assets/screenfull-2.png" alt="screen-full"
                            @click="onScreenFullClick" v-show="isScreenFull" />
                    </a-tooltip>
                    <a-tooltip mini content="返回首页">
                        <img  @click="onReloadClick" src="../assets/refresh.png" alt="refresh" />
                    </a-tooltip>
                </div>
                
            </div>
        </div>
        <div class="back" @click="onReloadClick" v-if="jumpTitle.length">
           <img  class="back-icon" src="../assets/leftIcon.png" alt="">
        </div>
        <div class="div_iframe">
            <iframe ref="iframe" id="iframe" class="iframe" :src="iframePath" frameborder="0"></iframe>
        </div>
    </div>
</template>
<script setup>
    import { reactive, toRefs, onDeactivated, onMounted, ref } from 'vue'
    import { Vue3SeamlessScroll } from 'vue3-seamless-scroll'
    import screenFull from 'screenfull';
    import dayjs from 'dayjs';

    const dayArr = [
        '星期一',
        '星期二',
        '星期三',
        '星期四',
        '星期五',
        '星期六',
        '星期日'
    ]

    const state = reactive({
        nowTime:'',
        isScreenFull:screenFull.isFullscreen,
        title:'中国建筑国际集团',
        dataList:[
            {
                title:'测试测试测试数据',
                date:Date.now()
            }
        ],
        isScroll:false,
        jumpTitle:'',
        iframePath:'https://www.c-smart.cn/home/login/index.html#/code-login-single?code=b1c56f88212644fe9d041e1ec1945646&needHeader=false&needBg=false'
    })


    const timer = setInterval(() => {
        state.nowTime = `${dayjs().format('YYYY-MM-DD')} ${dayArr[dayjs().day() - 1]}  ${dayjs().format('HH:mm:ss')}`
    }, 1000);

    const onScreenFullClick = () => {
        if (!screenFull.isEnabled) {
            console.error('暂不支持全屏');
            return;
        }
        screenFull.toggle();
        screenFull.on('change', () => {
            state.isScreenFull = screenFull.isFullscreen;
        });
    }

    const onReloadClick = () => {
        state.iframePath = 'about'
        setTimeout(() => {
            state.jumpTitle = ''
            state.iframePath = 'https://www.c-smart.cn/home/login/index.html#/code-login-single?code=b1c56f88212644fe9d041e1ec1945646&needHeader=false&needBg=false'
        }, 300);
    }
    

    onDeactivated(()=>{
        clearInterval(timer)
    })

    onMounted(()=>{
        window.onmessage = ((e)=>{
            const { data } = e
            console.log(data,'------------sdfdsafs')
            if (data.current) {
                state.jumpTitle = data.current.address
            }
        })        
    })
    

    const {
        nowTime,
        isScreenFull,
        title,
        dataList,
        jumpTitle,
        iframePath
    } = toRefs(state)
</script>
<style lang="scss" scoped>
.container {
    width: 100vw;
    height: 100vh;
    background-image: url('../assets/bg.png');
    background-size: 100% 100%;
    position: relative;
}
.div_iframe{
    top: -1%;
    position: absolute;
    z-index: 20;
    pointer-events: auto;
}
.iframe {
    width: 101vw;
    height: 100vh;
    top: 1%;
}
.back {
    width: 40px;
    height: 40px;
    background: rgba(0,0,0,.5);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    position: fixed;
    top:55%;
    left:0;
    cursor: pointer;
    z-index: 100;
    &-icon {
        width: 15px;
        height: 15px;
    }
}
.c-header {
    
    &-home_header {
        // position: relative;
        z-index: 100;
        display: flex;
        width: 100%;
        height: .85rem;
        // z-index: 10;
        background-image:url('../assets/head-bg.png');
        background-size: 100% 100%;
        .left {
            width: 28.9%;

            .now-time {
                display: flex;
                align-items: center;
                height: 0.32rem;
                padding-left: 0.17rem;
                font-size: 0.14rem;
                font-family: Source Han Sans CN;
                font-weight: 400;
                color: #91e8ff;
            }
        }

        .org-name-area {
            position: relative;
            width: 42.2%;
            display: flex;
            // margin-top: 0.18rem;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            .org-name-area-main {
                width: 100%;
                display: flex;
                padding: 0 15%;
                box-sizing: border-box;
                justify-content:center;
                align-items: center;
            }

            .org-name {
                font-size: 0.34rem;
                font-family: Source Han Sans CN;
                font-weight: 500;
                color: #78d2ed;
                white-space: nowrap;
                text-align: center;
                background: linear-gradient(180deg, #ffffff 0%, #63deff 84.8388671875%);
                -webkit-background-clip: text;
                -webkit-text-fill-color: transparent;
            }

            .menu-btn {
                width: 0.45rem;
                height: 0.6rem;
                cursor: pointer;
                margin: .03rem 0.05rem;
                img{
                    margin-bottom: 0.05rem;
                }

                &:hover {
                    opacity: 0.8;
                }
            }

            .switch-org {
                @extend .menu-btn;
            }

            .menu-area {
                position: absolute;
                width: 100%;
                height: 3.01rem;
                background-size: 100% 100%;
                left: 0;
                top: 0.25rem;
                padding: 0.5rem 1rem;

                display: flex;

                .menu {
                    flex: 1;
                    display: flex;
                    justify-content: center;
                    cursor: pointer;
                    transition: 0.2s;

                    img {
                        width: 0.6rem;
                        height: 0.6rem;
                    }
                    &:nth-child(1) {
                        margin-top: 0.3rem;
                    }
                    &:nth-child(2) {
                        margin-top: 0.75rem;
                    }
                    &:nth-child(3) {
                        margin-top: 1.2rem;
                    }
                    &:nth-child(4) {
                        margin-top: 1.4rem;
                    }
                    &:nth-child(5) {
                        margin-top: 1.45rem;
                    }
                    &:nth-child(6) {
                        margin-top: 1.4rem;
                    }
                    &:nth-child(7) {
                        margin-top: 1.2rem;
                    }
                    &:nth-child(8) {
                        margin-top: 0.75rem;
                    }
                    &:nth-child(9) {
                        margin-top: 0.3rem;
                    }

                    &:hover {
                        transform: scale(1.1);
                    }
                }
            }
        }

        .right {
            width: 28.7%;

            .buttons {
                position: relative;
                z-index: 21;
                height: 0.32rem;
                display: flex;
                align-items: center;
                justify-content: end;
                margin-right: 0.17rem;
                img {
                    height: 0.18rem;
                    width: auto;
                    margin-left: 0.1rem;
                    cursor: pointer;
                    transition: 0.2s;

                    &:hover {
                        transform: scale(1.1);
                    }
                }
            }
        }
    }
}
</style>