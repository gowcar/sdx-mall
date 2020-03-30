<template>
    <view class="container">
        <!--tabbar-->
        <view class="tui-tabbar">
            <block v-for="(item,index) in tabbar" :key="index">
                <view class="tui-tabbar-item" :class="[current==index?'tui-item-active':'']" :data-index="index" @tap="tabbarSwitch">
                    <view :class="[index==0?'tui-ptop-4':'']">
                        <tui-icon :name="current==index?item.icon+'-fill':item.icon" :color="current==index?'#E41F19':'#666'" :size="item.size"></tui-icon>
                    </view>
                    <view class="tui-scale">{{item.text}}</view>
                </view>
            </block>
        </view>
        <!--tabbar-->
        <!--header-->
        <view class="tui-header">
            <view class="tui-category" hover-class="opcity" :hover-stay-time="150" @tap="classify">
                <tui-icon name="manage-fill" color="#fff" :size="22"></tui-icon>
                <view class="tui-category-scale">分类</view>
            </view>
            <view class="tui-rolling-search">
                <!-- #ifdef APP-PLUS || MP -->
                <icon type="search" :size='13' color='#999'></icon>
                <!-- #endif -->
                <!-- #ifdef H5 -->
                <view>
                    <tui-icon name="search" :size='16' color='#999'></tui-icon>
                </view>
                <!-- #endif -->
                <swiper vertical autoplay circular interval="8000" class="tui-swiper">
                    <swiper-item v-for="(item,index) in hotSearch" :key="index" class="tui-swiper-item" @tap="search">
                        <view class="tui-hot-item">{{item}}</view>
                    </swiper-item>
                </swiper>
            </view>
        </view>
        <!--header-->
        <view class="tui-header-banner">
            <view class="tui-hot-search">
                <view>热搜</view>
                <view class="tui-hot-tag" @tap="search">自热火锅</view>
                <view class="tui-hot-tag" @tap="search">华为手机</view>
                <view class="tui-hot-tag" @tap="search">有机酸奶</view>
                <view class="tui-hot-tag" @tap="search">苹果手机</view>
            </view>
            <view class="tui-banner-bg">
                <view class="tui-primary-bg tui-route-left"></view>
                <view class="tui-primary-bg tui-route-right"></view>
                <!--banner-->
                <view class="tui-banner-box">
                    <swiper :indicator-dots="true" :autoplay="true" :interval="5000" :duration="150" class="tui-banner-swiper"
                            :circular="true" indicator-color="rgba(255, 255, 255, 0.8)" indicator-active-color="#fff">
                        <swiper-item v-for="(item,index) in banner" :key="index" @tap.stop="detail">
                            <image :src="'../../static/images/mall/banner/'+item" class="tui-slide-image" mode="scaleToFill" />
                        </swiper-item>
                    </swiper>
                </view>
            </view>
        </view>

        <view class="tui-product-category">
            <view class="tui-category-item" v-for="(item,index) in category" :key="index" :data-key="item.name" @tap="more">
                <image :src="'../../static/images/mall/category/'+item.img" class="tui-category-img" mode="scaleToFill"></image>
                <view class="tui-category-name">{{item.name}}</view>
            </view>
        </view>

        <view class="tui-product-box tui-pb-20 tui-bg-white">
            <view class="tui-group-name" @tap="more">
                <text>新人专享</text>
                <tui-icon name="arrowright" :size="20" color="#555"></tui-icon>
            </view>
            <view class="tui-activity-box" @tap="detail">
                <image src="../../static/images/mall/activity/activity_1.jpg" class="tui-activity-img" mode="widthFix"></image>
                <image src="../../static/images/mall/activity/activity_2.jpg" class="tui-activity-img" mode="widthFix"></image>
            </view>
        </view>

        <view class="tui-product-box tui-pb-20 tui-bg-white">
            <view class="tui-group-name" @tap="more">
                <text>新品推荐</text>
                <tui-icon name="arrowright" :size="20" color="#555"></tui-icon>
            </view>
            <view class="tui-new-box">
                <view class="tui-new-item" :class="[index!=0 && index!=1 ?'tui-new-mtop':'']" v-for="(item,index) in newProduct"
                      :key="index" @tap="detail">
                    <image :src="'../../static/images/mall/new/'+(item.type==1?'new':'discount')+'.png'" class="tui-new-label" v-if="item.isLabel"></image>
                    <view class="tui-title-box">
                        <view class="tui-new-title">{{item.name}}</view>
                        <view class="tui-new-price">
                            <text class="tui-new-present">￥{{item.present}}</text>
                            <text class="tui-new-original">￥{{item.original}}</text>
                        </view>
                    </view>
                    <image :src="'../../static/images/mall/new/'+item.pic" class="tui-new-img"></image>
                </view>
            </view>
        </view>

        <view class="tui-product-box">
            <view class="tui-group-name">
                <text>热门推荐</text>
            </view>
            <view class="tui-product-list">
                <view class="tui-product-container">
                    <block v-for="(item,index) in productList" :key="index" v-if="(index+1)%2!=0">
                        <!--商品列表-->
                        <view class="tui-pro-item" hover-class="hover" :hover-start-time="150" @tap="detail">
                            <image :src="'../../static/images/mall/product/'+item.img+'.jpg'" class="tui-pro-img" mode="widthFix" />
                            <view class="tui-pro-content">
                                <view class="tui-pro-tit">{{item.name}}</view>
                                <view>
                                    <view class="tui-pro-price">
                                        <text class="tui-sale-price">￥{{item.sale}}</text>
                                        <text class="tui-factory-price">￥{{item.factory}}</text>
                                    </view>
                                    <view class="tui-pro-pay">{{item.payNum}}人付款</view>
                                </view>
                            </view>
                        </view>
                        <!--商品列表-->
                        <!-- <template is="productItem" data="{{item,index:index}}" /> -->
                    </block>
                </view>
                <view class="tui-product-container">
                    <block v-for="(item,index) in productList" :key="index" v-if="(index+1)%2==0">
                        <!--商品列表-->
                        <view class="tui-pro-item" hover-class="hover" :hover-start-time="150" @tap="detail">
                            <image :src="'../../static/images/mall/product/'+item.img+'.jpg'" class="tui-pro-img" mode="widthFix" />
                            <view class="tui-pro-content">
                                <view class="tui-pro-tit">{{item.name}}</view>
                                <view>
                                    <view class="tui-pro-price">
                                        <text class="tui-sale-price">￥{{item.sale}}</text>
                                        <text class="tui-factory-price">￥{{item.factory}}</text>
                                    </view>
                                    <view class="tui-pro-pay">{{item.payNum}}人付款</view>
                                </view>
                            </view>
                        </view>
                        <!--商品列表-->
                        <!-- <template is="productItem" data="{{item,index:index}}" /> -->
                    </block>
                </view>
            </view>
        </view>

        <!--加载loadding-->
        <tui-loadmore :visible="loadding" :index="3" type="red"></tui-loadmore>
        <!-- <tui-nomore visible="{{!pullUpOn}}"></tui-nomore> -->
        <!--加载loadding-->
        <view class="tui-safearea-bottom"></view>
    </view>
</template>
<script>
    import tuiIcon from "@/components/icon/icon"
    import tuiTag from "@/components/tag/tag"
    import tuiLoadmore from "@/components/loadmore/loadmore"
    import tuiNomore from "@/components/nomore/nomore"
    export default {
        components: {
            tuiIcon,
            tuiTag,
            tuiLoadmore,
            tuiNomore
        },
        data() {
            return {
                current: 0,
                tabbar: [{
                    icon: "home",
                    text: "首页",
                    size: 21
                }, {
                    icon: "category",
                    text: "分类",
                    size: 24
                }, {
                    icon: "cart",
                    text: "购物车",
                    size: 22
                }, {
                    icon: "people",
                    text: "我的",
                    size: 24
                }],
                hotSearch: [
                    "休闲零食",
                    "自热火锅",
                    "小冰箱迷你"
                ],
                banner: [
                    "1.jpg",
                    "2.jpg",
                    "3.jpg",
                    "4.jpg",
                    "5.jpg"
                ],
                category: [{
                    img: "1.jpg",
                    name: "短袖T恤"
                }, {
                    img: "2.jpg",
                    name: "足球"
                }, {
                    img: "3.jpg",
                    name: "运动鞋"
                }, {
                    img: "4.png",
                    name: "中老年"
                }, {
                    img: "5.png",
                    name: "甜美风"
                }, {
                    img: "6.jpg",
                    name: "鱼尾裙"
                }, {
                    img: "7.jpg",
                    name: "相机配件"
                }, {
                    img: "8.jpg",
                    name: "护肤套装"
                }, {
                    img: "9.jpg",
                    name: "单肩包"
                }, {
                    img: "10.jpg",
                    name: "卫衣"
                }],
                newProduct: [{
                    name: "时尚舒适公主裙高街修身长裙",
                    present: 198,
                    original: 298,
                    pic: "1.jpg",
                    type: 1,
                    isLabel: true
                }, {
                    name: "高街修身雪纺衫",
                    present: 398,
                    original: 598,
                    pic: "2.jpg",
                    type: 2,
                    isLabel: true
                }, {
                    name: "轻奢商务上衣",
                    present: 99,
                    original: 199,
                    pic: "3.jpg",
                    type: 1,
                    isLabel: true
                }, {
                    name: "品质牛皮婚鞋牛皮婚鞋品质就是好",
                    present: 99,
                    original: 199,
                    pic: "5.jpg",
                    type: 1,
                    isLabel: true
                }, {
                    name: "轻奢时尚大包限时新品推荐",
                    present: 99,
                    original: 199,
                    pic: "6.jpg",
                    type: 1,
                    isLabel: false
                }, {
                    name: "高街修身长裙",
                    present: 999,
                    original: 1299,
                    pic: "4.jpg",
                    type: 2,
                    isLabel: true
                }],
                productList: [{
                    img: 1,
                    name: "欧莱雅（LOREAL）奇焕光彩粉嫩透亮修颜霜 30ml（欧莱雅彩妆 BB霜 粉BB 遮瑕疵 隔离）",
                    sale: 599,
                    factory: 899,
                    payNum: 2342
                },
                    {
                        img: 2,
                        name: "德国DMK进口牛奶  欧德堡（Oldenburger）超高温处理全脂纯牛奶1L*12盒",
                        sale: 29,
                        factory: 69,
                        payNum: 999
                    },
                    {
                        img: 3,
                        name: "【第2支1元】柔色尽情丝柔口红唇膏女士不易掉色保湿滋润防水 珊瑚红",
                        sale: 299,
                        factory: 699,
                        payNum: 666
                    },
                    {
                        img: 4,
                        name: "百雀羚套装女补水保湿护肤品",
                        sale: 1599,
                        factory: 2899,
                        payNum: 236
                    },
                    {
                        img: 5,
                        name: "百草味 肉干肉脯 休闲零食 靖江精制猪肉脯200g/袋",
                        sale: 599,
                        factory: 899,
                        payNum: 2399
                    },
                    {
                        img: 6,
                        name: "短袖睡衣女夏季薄款休闲家居服短裤套装女可爱韩版清新学生两件套 短袖粉色长颈鹿 M码75-95斤",
                        sale: 599,
                        factory: 899,
                        payNum: 2399
                    },
                    {
                        img: 1,
                        name: "欧莱雅（LOREAL）奇焕光彩粉嫩透亮修颜霜",
                        sale: 599,
                        factory: 899,
                        payNum: 2342
                    },
                    {
                        img: 2,
                        name: "德国DMK进口牛奶",
                        sale: 29,
                        factory: 69,
                        payNum: 999
                    },
                    {
                        img: 3,
                        name: "【第2支1元】柔色尽情丝柔口红唇膏女士不易掉色保湿滋润防水 珊瑚红",
                        sale: 299,
                        factory: 699,
                        payNum: 666
                    },
                    {
                        img: 4,
                        name: "百雀羚套装女补水保湿护肤品",
                        sale: 1599,
                        factory: 2899,
                        payNum: 236
                    }
                ],
                pageIndex: 1,
                loadding: false,
                pullUpOn: true
            }
        },
        methods: {
            tabbarSwitch: function(e) {
                let index = e.currentTarget.dataset.index;
                this.current = index;
                if (index != 0) {
                    if (index == 1) {
                        this.classify();
                    } else if (index == 2) {
                        uni.navigateTo({
                            url: '../mall-extend/shopcart/shopcart'
                        })
                    } else {
                        uni.navigateTo({
                            url: '../mall-extend/my/my'
                        })
                    }
                }
            },
            detail: function() {
                uni.navigateTo({
                    url: '../productDetail/productDetail'
                })
            },
            classify: function() {
                uni.navigateTo({
                    url: '/pages/navbar-2/navbar-2'
                })

            },
            more: function(e) {
                let key = e.currentTarget.dataset.key || "";
                uni.navigateTo({
                    url: '../productList/productList?searchKey=' + key
                })

            },
            search: function() {
                uni.navigateTo({
                    url: '../news-search/news-search'
                })
            }
        },
        onPullDownRefresh: function() {
            let loadData = JSON.parse(JSON.stringify(this.productList));
            loadData = loadData.splice(0, 10)
            this.productList = loadData;
            this.pageIndex = 1;
            this.pullUpOn = true;
            this.loadding = false
            uni.stopPullDownRefresh()
        },
        onReachBottom: function() {
            if (!this.pullUpOn) return;
            this.loadding = true;
            if (this.pageIndex == 4) {
                this.loadding = false;
                this.pullUpOn = false
            } else {
                let loadData = JSON.parse(JSON.stringify(this.productList));
                loadData = loadData.splice(0, 10)
                if (this.pageIndex == 1) {
                    loadData = loadData.reverse();
                }
                this.productList = this.productList.concat(loadData);
                this.pageIndex = this.pageIndex + 1;
                this.loadding = false
            }
        }
    }
</script>
<style>
    @import '@/static/mall.css';
</style>
