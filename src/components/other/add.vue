<template>
    <div class="scroll-list-wrap">
        <cube-scroll ref="scroll">
            <template>
                <div class="list">
                    <label class="form-group item item-input ">
                        <span>物品名称：</span>
                        <input type="text" v-model="other.otherName">
                    </label>
                    <label class="form-group item item-input ">
                        <span>官方价格：</span>
                        <input type="text" v-model="other.originalPrice">
                    </label>
                    <label class="form-group item item-input ">
                        <span>出售价格：</span>
                        <input type="text" v-model="other.presentPrice">
                    </label>
                    <label class="form-group item item-input ">
                        <span>购买日期：</span>
                        <input type="text" v-model="other.buyDate">
                    </label>
                    <label class="form-group item item-input ">
                        <span>是否有发票：</span>
                        <input type="text" v-model="other.hasInvoice">
                    </label>
                    <label class="form-group item item-input ">
                        <span>联系方式：</span>
                        <input type="text" v-model="other.phone">
                    </label>
                    <label class="form-group item item-input ">
                        <span>描述：</span>
                        <input type="text" v-model="other.des">
                    </label>
                    <file-com ref="refFiles"></file-com>
                    <button style="width: 100%" class="button button-positive" @click="saveData">发布</button>
                </div>
            </template>
        </cube-scroll>
    </div>
</template>

<script>
    import FileCom from '../common/FileCom'
    import $ from 'jquery'
    let _that;
    export default {
        name: "add",
        data() {
            return {
                other: {
                    otherName: '',
                    originalPrice: '20',
                    presentPrice: '15',
                    buyDate: '2013-12-12',
                    hasInvoice: '1',
                    otherPic: '',
                    weiXin: '',
                    phone: '',
                    des: '',
                },
                urls:[],
            }
        },
        components: {
            'file-com': FileCom,
        },
        created(){
            _that=this;
            this.other.id=this.$route.query.id;
            if(parseInt(this.other.id)!==0)
                this.initData();
        },
        mounted() {
            this.$(".scroll-list-wrap").height = this.$(".scroll-list-wrap").height(screen.availHeight - this.$(".tabs-icon-top", window.parent.parent.document).height()) + 80;
        },
        methods:{
            initData(){
                this.$http.post('/other/getById/'+this.other.id).then((res)=>{
                    _that.other=res.data.page.info;
                    _that.other.otherType=1;//先不做处理后面要删除
                    var arr=_that.other.otherPic.split(',');
                    $.each(arr,(index,item)=>{
                        _that.urls.push({url:_that.$file(item)});
                    })
                })
            },
            saveData(){
                let  url='/other/save';
                if(this.other.id!=0)
                    url='/other/update';
                this.$save(url,this.other,this.$refs.refFiles.files,(msg)=>{
                    console.log(msg);
                })
            }
        }
    }
</script>


