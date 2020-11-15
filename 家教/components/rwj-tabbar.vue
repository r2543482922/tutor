 <template>  
    <view class="tabbar-container">  
        <block>  
            <view class="tabbar-item" v-for="(item,index) in tabbarList" :class="[item.centerItem ? ' center-item' : '']" @click="changeItem(item)">  
                <view class="item-top">  
                    <image  :src="currentItem==item.id?item.selectIcon:item.icon"></image>  
                </view>  
                <view class="item-bottom" :class="[currentItem==item.id ? 'item-active' : '']">  
                    <text>{{item.text}}</text>  
                </view>  
            </view>  
        </block>  

    </view>  
</template>  

<script>  
    export default {  
        props:{  
            currentPage: {  
                type: Number,  
                default: 0  
            }  
        },  
        data() {  
            return {  
                currentItem: 0,  
                tabbarList: [  
                                   {  
                                       id: 0,  
                                      "path": "pages/mianpage/mainpage",
                                      "icon": "static/mainpage/homepage-icon-h.png",
                                      "selectIcon": "static/mainpage/homepage-icon.png",
                                      "text": "Ê×Ò³" ,
                                       centerItem: false  
                                   },{  
                                       id: 1,  
                                       "path": "pages/register/index",
                                       "icon": "static/mainpage/icon-fabu.png",
                                       "selectIcon": "static/mainpage/icon-fabu.png" ,
                                       centerItem: true  
                                   },{  
                                       id: 2,  
                                       "path": "pages/login/login",
                                       "icon": "static/mainpage/homepage-gr-icon.png",
                                       "selectIcon": "static/mainpage/homepage-gr-icon-2.png",
                                       "text": "¸öÈË",
                                       centerItem: false  
                                   }
                               ]  
               
                           };  
                       },  
        mounted(){  
            this.currentItem = this.currentPage;  
            uni.hideTabBar();  
        },  
        methods: {  
            changeItem(item){  
                let _this = this;  
                //_this.currentItem = item.id;  
                uni.switchTab({  
                    url: item.path  
                });  
            }  
        }  
    }  
</script>  
<style>  
        view{  
               padding: 0;  
               margin: 0;  
               box-sizing: border-box;  
        }  
    .tabbar-container{  
        position: fixed;  
        bottom: 0;  
        left: 0;  
        width: 100%;  
        height: 110rpx;  
        box-shadow: 0 0 5px #999;  
        display: flex;  
        align-items: center;  
        padding: 5rpx 0;  
        color: #999999;  
    }  
    .tabbar-container .tabbar-item{  
        width: 20%;  
        height: 100rpx;  
        display: flex;  
        flex-direction: column;  
        justify-content: center;  
        align-items: center;  
        text-align: center;  
    }  
    .tabbar-container .item-active{  
        color: #66CCFF;  
    }  
    .tabbar-container .center-item{  
        display: block;  
        position: relative;  
    }  
    .tabbar-container .tabbar-item .item-top{  
        width: 70rpx;  
        height: 70rpx;  
        padding: 10rpx;  
    }  
    .tabbar-container .center-item .item-top{  
        flex-shrink: 0;  
        width: 100rpx;  
        height: 100rpx;  
        position: absolute;  
        top: -50rpx;  
        left: calc(50% - 50rpx);  
        border-radius: 50%;  
        box-shadow: 0 0 5px #999;  
        background-color: #ffffff;  
    }  
    .tabbar-container .tabbar-item .item-top image{  
        width: 100%;  
        height: 100%;  
    }  
    .tabbar-container .tabbar-item .item-bottom{  
        font-size: 28rpx;  
        width: 100%;  
    }  
    .tabbar-container .center-item .item-bottom{  
        position: absolute;  
        bottom: 5rpx;  
    }  
</style>  