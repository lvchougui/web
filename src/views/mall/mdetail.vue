<template>
    
    <div class="content-bg" style="padding-top:50px;">
        <div style="font-size:28px;color:black;text-align:center;font-family: KaiTi,KaiTi_GB2312 ! important;font-weight:900;margin: 0 auto;" >作品信息</div>
        <div style="height:3px;background-color:#272727;margin-top:10px;width:100%;" ></div>
        <div class="mcontent" >
            <div style="position:relative;"></dir><img :src="detail.cover+'?imageView2/1/w/1000/h/1000/interlace/1'" style="border:10px solid #efefef;width:100%;height:auto;">
                <img src="../../assets/img/syyj/jieyuan.png" style="width:110px;position:absolute;right:20px;top:40px;transform:rotate(30deg)" v-if="detail.is_sold==1">
            </div>
            <div class="info-detail" style="margin:0;">
                <div style="width:100%;text-align:center;font-size:24px;font-family:KaiTi,KaiTi_GB2312 ! important;font-weight:600;color:black;margin-top:10px;border-bottom:1px dashed #000;">{{detail.name}}</div>
                <div style="margin-top:25px;font-size:15px;">作品材质：{{detail.p_material}}</div>
                <div style="margin-top:5px;font-size:15px;">作品尺寸：{{detail.p_size}}</div>
                <div style="margin-top:5px;font-size:15px;">作品重量：{{detail.p_weight}}</div>
                <div style="margin-top:20px;word-break: break-all;display: -webkit-box;-webkit-line-clamp: 3; -webkit-box-orient: vertical;font-size:15px;">作品简介：{{detail.summary}}</div>
                <div id="qrcode" style="width:150px;height:150px;margin:30px auto;"></div>
                
            </div>
            <div style="height:2px;background-color:#272727;margin-top:20px;" ></div>
            <div class="product-info-desc" style="margin-top:20px;word-break">{{{detail.detail}}}</div>
        </div>
    </div>
    
    
</template>
<style>
    .product-cover {
        width: 100%;
        height: auto;
    }
    .content-bg{
        width: 100%;
        height: auto;
        background: url('../../assets/img/syyj/mainbg.jpg');
        padding-top: 150px;
    }
    .mcontent{
        display:flex;
        flex-direction:column;
        background-color:white;
        margin-top: 30px;
        padding: 20px;
    }
    .product-info{
        display: flex;
        flex-direction: row;
        padding: 40px;
        width: 100%;
    }
    /*.product-info img{
        width: 420px;
        height: 420px;
    }*/
    .info-detail{
        flex: 1;
        margin-left: 60px;
        position: relative;
        /*display: flex;*/
        /*flex-direction: column;*/
    }
    .product-info-desc img{
        max-width: 100%;
        text-align: center;
        height: auto;
        margin-top: 5px;
    }
</style>
<script>

    export default {
        components: {
        },
        ready: function() {
            this.id = this.$route.params.id;
            this.getDefaultData();
        },
        data: function() {
            return {
                id: '',
                detail: {}
                
            }
        },
        mounted: function () {
          this.$nextTick(function () {
            
            this.qrcode();
        })
      },
      methods: {

        qrcode:function () {
            $("#qrcode").qrcode({
                text: window.location.href,
                width:150,
                height:150
            });
        },
        //得到默认的数据
        getDefaultData: function() {
            var url = "http://47.94.206.22:3001/api/product/getProductDetail/"+this.id;
            this.$http.get(
                url
                )
            .then((res) => {
               if (typeof(res.data) == 'string') {
                res.data = JSON.parse(res.data);
            }
            if (res.status == 200) {
                this.detail = res.data;
                this.qrcode();
            };
        })
            .catch((res) => {
                log("error:" + JSON.stringify(res));
            });
        },

    }
}
</script>
