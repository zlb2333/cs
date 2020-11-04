<template>
    <div class="left-nav">
        <ul>
            <li>
                <i class="icon iconfont icon-goumai"></i>
                <div>收银</div>
            </li>
             <li>
                <i class="icon iconfont icon-dianpu"></i>
                <div>店铺</div>
            </li>
             <li>
                <i class="icon iconfont icon-hanbao"></i>
                <div>商品</div>
            </li>
             <li>
                <i class="icon iconfont icon-huiyuanqia"></i>
                <div>会员</div>
            </li>
             <li>
                <i class="icon iconfont icon-tongji "></i>
                <div>统计</div>
            </li>
              <li>
                <i class="icon iconfont icon-zuanshi "></i>
                <div>设置</div>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name:'leftNav'
    
}
</script>

<style >
.left-nav{
    color: #fff;
    font-size: 10px;
    height: 100%;
    background-color: #1d8ce0;
    float: left;
    width: 5%;

}
.iconfont{
    font-size: 24px;
}
.left-nav>ul{
    padding: 0;
    margin: 0;
}
.left-nav>ul>li{
    text-align: center;
    list-style: none;
    border-bottom: 1px solid #20a2ff;
    padding: 10px;
}
</style>