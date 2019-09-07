<template>
    <div class="systemManage">
        <!-- 可视的图标菜单 -->
        <span class="itemBlock" v-for="(item,index) in manageList" :key="index" @click.stop="showBlock(item.clickEv)">
            <span :class="item.icon" class="iconfontItem"></span>
            <span class="blockName">{{item.titleName}}</span>
        </span>
        <!-- 根据不同不同的图标展示不同的弹窗 -->
        <div class="optionSetting" ref="optionSetting" >
            <div class="settingContent">
                <!-- 弹窗的标题 -->
                <div class="fixedTitle">
                    <h2 class="titleName">选项设置</h2>
                    <div class="rightBtn">
                        <span class="questionMack iconfont icon-wenhao"></span>
                        <span class="close iconfont icon-shanchu" @click="close('optionSetting')" ></span>
                    </div>
                </div>
                <!-- 弹窗的内容 -->
                <div class="blockContentPadding">
                    <div class="blockContent">
                        <!-- tab -->
                        <div class="switchBtnGroup" >
                            <!-- 单个的选项卡 -->
                            <button class="btnItem" :class="switchIndex == index ? 'linkSwitch' : '' " v-for="(item,index) in switchTabList" :key="index" @click="switchTab(item.switchKey,index)">{{item.name}}</button>
                        </div>
                        <div class="switchContent">
                            <div class="overflowBox">
                                <form  class="publicData">
                                    <fieldset style="border:1px solid gray;">
                                        <legend style="margin-left:10px;">基础数据</legend>
                                        <div class="lineDivi">
                                            <label class="labelItem">
                                                <input type="checkbox" v-model="wldwflgl" disabled/>往来单位分类管理
                                            </label>
                                            <label class="labelItem">
                                                <input type="checkbox" v-model="chflgl" disabled/>存货分类管理
                                            </label>
                                        </div>
                                        <div class="lineDivi">
                                            <label class="labelItem">
                                                <input type="checkbox" v-model="xmdaflgl" disabled/>项目档案分类管理
                                            </label>
                                            <label class="labelItem">
                                                <input type="checkbox" v-model="wldwflgl" disabled/>往来单位分类管理
                                            </label>
                                        </div>
                                        <div class="lineDivi">
                                            <span class="spanItem">往来单位重复控制方式</span>
                                            <div class="labelItem radiogroup" >
                                                <label class="labelItem ">
                                                    <input type="radio" v-model="tip"  name='controlWay'/>提示
                                                </label>
                                                <label class="labelItem">
                                                    <input type="radio" v-model="forbid"  name='controlWay'/>禁止
                                                </label>
                                            </div>
                                        </div>
                                        <div class="lineDivi">
                                            <span style="margin:5px 0;" class="spanItem">往来单位重复检查项</span>
                                        </div>
                                        <div class="lineDivi controlItemBox">
                                            <label class="labelItem">
                                                <input type="checkbox" v-model="contolItem.name"  name="contolItem"/>名称
                                            </label>
                                            <label class="labelItem">
                                                <input type="checkbox" v-model="contolItem.abbr"  name="contolItem"/>简称
                                            </label>
                                            <label class="labelItem">
                                                <input type="checkbox" v-model="contolItem.phone"  name="contolItem"/>手机号
                                            </label>
                                            <label class="labelItem">
                                                <input type="checkbox" v-model="contolItem.telPhone"  name="contolItem"/>电话
                                            </label>
                                        </div>
                                        <div class="lineDivi">
                                            <label class="labelItem">
                                                <input type="checkbox" v-model="wlqddzj"  />物料清单电子件重复控制
                                            </label>
                                        </div>
                                        <!-- wlqddzj -->
                                    </fieldset>
                                </form>
                                <form class="dataPrecision">
                                    <fieldset style="border:1px solid gray;">
                                        <legend style="margin-left:10px;">数据精度</legend>
                                        <div class="lineDivi">
                                            <!-- conversion rate; -->
                                            <div class="labelItem">
                                                <span class="spanTitle">换算小数位</span>
                                                <select v-model="conversionRatePrecision" class="selectItem">
                                                    <option v-for="(item,index) in PrecisionList" :value="item" :key="index">{{item}}</option>
                                                </select>
                                            </div>
                                            <div class="labelItem rightLabelItem">
                                                <span class="spanTitle" >数量小数位</span>
                                                <select v-model="qualityPrecision" class="selectItem">
                                                    <option v-for="(item,index) in PrecisionList" :value="item" :key="index">{{item}}</option>
                                                </select>
                                                <label class="labelItem TurnZero">
                                                    <input type="checkbox" v-model="qualityPrecisionTurnZero" />舍零
                                                </label>
                                            </div>
                                        </div>
                                        <div class="lineDivi">
                                            <!-- conversion rate; -->
                                            <div class="labelItem">
                                                <span class="spanTitle " >单价整数位</span>
                                                <select v-model="singlePriceIntegerPrecision" class="selectItem">
                                                    <option v-for="(item,index) in PrecisionList" :value="item" :key="index">{{item}}</option>
                                                </select>
                                            </div>
                                            <div class="labelItem rightLabelItem">
                                                <span class="spanTitle " >单价小数位</span>
                                                <select v-model="qualityPrecision" class="selectItem">
                                                    <option v-for="(item,index) in PrecisionList" :value="item" :key="index">{{item}}</option>
                                                </select>
                                                <label class="labelItem TurnZero">
                                                    <input type="checkbox" v-model="singlePriceDecimalsPrecisionTurnZero" />舍零
                                                </label>
                                            </div>
                                        </div>
                                        <div class="lineDivi">
                                            <!-- conversion rate; -->
                                            <div class="labelItem">
                                                <span class="spanTitle" >发票单价整数位</span>
                                                <select v-model="InvoiceUnitPriceIntegerPrecision" class="selectItem">
                                                    <option v-for="(item,index) in PrecisionList" :value="item" :key="index">{{item}}</option>
                                                </select>
                                            </div>
                                            <div class="labelItem rightLabelItem">
                                                <span class="spanTitle  " >发票单价小数位</span>
                                                <select v-model="InvoiceUnitPriceDecimalsPrecision" class="selectItem">
                                                    <option v-for="(item,index) in PrecisionList" :value="item" :key="index">{{item}}</option>
                                                </select>
                                                <div class="seizeSeat"></div>
                                            </div>
                                        </div>
                                        <div class="lineDivi">
                                            <!-- conversion rate; -->
                                            <div class="labelItem">
                                                <span class="spanTitle" >成品率小数位</span>
                                                <select v-model="productionYieldPrecision" class="selectItem">
                                                    <option v-for="(item,index) in PrecisionList" :value="item" :key="index">{{item}}</option>
                                                </select>
                                            </div>
                                            <div class="labelItem rightLabelItem">
                                                <span class="spanTitle  " >损耗率小数位</span>
                                                <select v-model="damagedRatePrecision" class="selectItem">
                                                    <option v-for="(item,index) in PrecisionList" :value="item" :key="index">{{item}}</option>
                                                </select>
                                                <div class="seizeSeat"></div>
                                            </div>
                                        </div>
                                        <div class="lineDivi">
                                            <!-- conversion rate; -->
                                            <div class="labelItem">
                                                <span class="spanTitle" >折旧率小数位</span>
                                                <select v-model="depreciationRatePrecision" class="selectItem">
                                                    <option v-for="(item,index) in PrecisionList" :value="item" :key="index">{{item}}</option>
                                                </select>
                                            </div>
                                            <div class="labelItem rightLabelItem">
                                                <span class="spanTitle " >汇率小数位</span>
                                                <select v-model="exchangeRatePrecision" class="selectItem">
                                                    <option v-for="(item,index) in PrecisionList" :value="item" :key="index">{{item}}</option>
                                                </select>
                                                <div class="seizeSeat"></div>
                                            </div>
                                        </div>
                                        <!-- wlqddzj -->
                                    </fieldset>
                                </form>
                                <form class="barcodeManagement" >
                                    <fieldset style="border:1px solid gray;">
                                        <legend style="margin-left:10px;">条形码管理</legend>
                                        <div class="lineDivi">
                                            <label class="labelItem">
                                                <input type="checkbox" v-model="barcode" />条形码管理
                                            </label>
                                            <label class="labelItem" v-if="barcode">
                                                <input type="checkbox" v-model="oneCodeGoods" />一码多物
                                            </label>
                                        </div>
                                        <div class="lineDivi" v-if="barcode">
                                            <span style="flex:1">条码方案标识位数</span>
                                            <div style="flex:1">
                                                <select v-model="barCodePrecision" class="selectItem" style="width:60px;">
                                                    <option v-for="(item,index) in PrecisionList" :value="item" :key="index">{{item}}</option>
                                                </select>
                                            </div>
                                        </div>
                                        <div class="lineDivi" v-if="barcode">
                                            <label class="labelItem" >
                                                <input type="checkbox" v-model="scanCombineDetail" />扫描条形码时相同细节合并
                                            </label>
                                        </div>
                                        <!---->
                                    </fieldset>
                                    <div class="lineDivi">
                                        <label class="labelItem singleItem">
                                            <input type="checkbox" v-model="multiMarket" />多营销机构
                                        </label>
                                    </div>
                                    <div class="lineDivi">
                                        <label class="labelItem singleItem">
                                            <input type="checkbox" v-model="receiptsInput" />单据二维录入
                                        </label>
                                    </div>
                                    <div class="lineDivi">
                                        <label class="labelItem singleItem">
                                            <input type="checkbox" v-model="deleteReceiptsLog" />删除单据记录详细日志
                                        </label>
                                    </div>
                                    <div class="lineDivi">
                                        <label class="labelItem singleItem">
                                            <input type="checkbox" v-model="ApprovalFlow" />审批流
                                        </label>
                                    </div>
                                    <div class="lineDivi">
                                        <label class="labelItem singleItem">
                                            <input type="checkbox" v-model="ContainName" />工作圈消息包含帐套名称
                                        </label>
                                    </div>
                                    <div class="lineDivi">
                                        <label class="labelItem singleItem">
                                            <input type="checkbox" v-model="DirectInvoicing" />与发票平台对接直接开票
                                        </label>
                                    </div>
                                    <div class="lineDivi">
                                        <label class="labelItem singleItem">
                                            <input type="checkbox" v-model="CloudPrint" />云打印
                                        </label>
                                    </div>
                                    <!-- -->
                                </form>
                                <form class="batchManagement">
                                    <fieldset style="border:1px solid gray;">
                                        <legend style="margin-left:10px;">批号及效期管理</legend>
                                        <div class="lineDivi">
                                            <label class="labelItem">
                                                <input type="checkbox" v-model="batch" />批号管理
                                            </label>
                                            <label class="labelItem" style="margin-left:-150px;" v-if="batch">
                                                <input type="checkbox" v-model="validateTerm" />有效期管理
                                            </label>
                                        </div>
                                        <div class="lineDivi" v-if="batch">
                                            <label class="labelItem">
                                                <input type="checkbox" v-model="batchNumberAutoProduce" />批号自动生成
                                            </label>
                                            <!--validateTerm  batchNumberAutoProduce -->
                                        </div>
                                        <div class="lineDivi">
                                            <fieldset class="ruleBlock" >
                                                <legend style="margin-left:10px;">自动生成规则</legend>
                                                <div class="tableContent" >
                                                    <div class="autoProduceBatchNumberTableTitle">
                                                        <span v-for="(item,index) in autoProduceBatchNumberTableList" :key="index" :style="{width:item.width}">{{item.title}}</span>
                                                    </div>
                                                    <table class="autoProduceBatchNumberTable"  >
                                                    <tr v-for="(item,index) in autoProduceBatchNumberData" :key="index" class="contentItem">
                                                        <td style="width:15%">{{index}}</td>
                                                        <td style="width:40%" >{{item.attr}}</td>
                                                        <td style="width:30%">
                                                            <input type="text" v-model="item.length" style="width:60px" class="length"/>
                                                        </td>
                                                        <td style="width:15%" >
                                                            <input type="checkbox" v-model="item.ifJoin"/>
                                                        </td>
                                                    </tr>
                                                </table>
                                                </div>
                                            </fieldset>
                                        </div>
                                    </fieldset>
                                </form>
                            </div>
                            <!-- 第一个框 -->
                        </div>
                        <h3 style="margin:10px 0;">相关说明:</h3>
                        <div class="explainWords"></div>
                        <div class="opBtnGroup">
                            <button @click="submit('optionSetting')">确定</button>
                            <button  @click="close('optionSetting')" >取消</button>
                        </div>
                    </div>
                </div>

            </div>
        </div>
    </div>
</template>
<script>
    export default {
        data () {
            return {
                //基础数据
                wldwflgl:true,//往来单位
                chflgl:true,//存货分类
                xmdaflgl:true,//项目档案
                //往来单位重复控制方式
                tip:true,//提示
                forbid:false,//禁止
                //重复控制项
                contolItem:{
                    name:true,//名称
                    abbr:true,//简称
                    phone:true,//手机号
                    telPhone:true,//电话
                },
                wlqddzj:false,//物料清单
                //数据精度
                PrecisionList:[0,1,2,3,4,5,6,7,8],
                conversionRatePrecision:'1',//换算率小数位
                qualityPrecision:'5',//数量小数位
                qualityPrecisionTurnZero:false,//数量小数位的舍零操作
                singlePriceIntegerPrecision:'7',//单价整数位
                singlePriceDecimalsPrecision:'2',//单价小数位
                singlePriceDecimalsPrecisionTurnZero:false,//单价小数位的舍零操作
                InvoiceUnitPriceIntegerPrecision:'6',//数据单价整数位，
                InvoiceUnitPriceDecimalsPrecision:'6',//数据单价小数位，
                productionYieldPrecision:'6',//成品率小数位
                damagedRatePrecision:'2',//损耗率 小数位
                depreciationRatePrecision:'4',//折旧率小数位
                exchangeRatePrecision:'4',//汇率小数位
                //条形码管理
                barcode:true,//条形码管理
                oneCodeGoods:false,//一码多物 
                barCodePrecision:'2',//条形码方案标识位数 、
                scanCombineDetail:true,
                //批号管理
                batch:true,
                validateTerm:false,
                batchNumberAutoProduce:false,
                //
                multiMarket:false,//多营销
                receiptsInput:false,//二维录入
                deleteReceiptsLog:false,//删除日志 
                ApprovalFlow:false,//审批流  
                ContainName:true,//包含帐套名称  
                DirectInvoicing:false,//直接开票 
                CloudPrint:false,//云打印
                //autoProduceBatchNumberTableList:表头
                autoProduceBatchNumberTableList:[
                    {title:'级数',width:'15%'},
                    {title:'属性',width:'40%'},
                    {title:'长度',width:'30%'},
                    {title:'参与',width:'15%'},
                ],
                autoProduceBatchNumberData:[
                    {attr:'单据日期',length:'',ifJoin:false},
                    {attr:'供应商编码',length:'',ifJoin:false},
                    {attr:'部门编码',length:'',ifJoin:false},
                    {attr:'仓库编码',length:'',ifJoin:false},
                    {attr:'数量',length:'',ifJoin:false},
                    {attr:'流水号',length:'',ifJoin:false},
                ],
                publicForm:[

                ],
                switchIndex:0,
                optionSetting:false,
                manageList:[
                    {icon:'iconfont icon-weixiujilu',clickEv:'optionSetting',titleName:'选项设置'},
                    {icon:'iconfont icon-weixiujilu',clickEv:'optionSetting',titleName:'选项设置'},
                    {icon:'iconfont icon-weixiujilu',clickEv:'optionSetting',titleName:'选项设置'},
                    {icon:'iconfont icon-weixiujilu',clickEv:'optionSetting',titleName:'选项设置'},
                    {icon:'iconfont icon-weixiujilu',clickEv:'optionSetting',titleName:'选项设置'},
                    {icon:'iconfont icon-weixiujilu',clickEv:'optionSetting',titleName:'选项设置'},
                    {icon:'iconfont icon-weixiujilu',clickEv:'optionSetting',titleName:'选项设置'},
                    {icon:'iconfont icon-weixiujilu',clickEv:'optionSetting',titleName:'选项设置'},
                    {icon:'iconfont icon-weixiujilu',clickEv:'optionSetting',titleName:'选项设置'},
                    {icon:'iconfont icon-weixiujilu',clickEv:'optionSetting',titleName:'选项设置'},
                ],
                switchTabList:[
                    {name:'公共',key:'public'},
                    {name:'采购',key:'purchase'},
                    {name:'往来/出纳',key:'cashier'},
                    {name:'销售',key:'sale'},
                    {name:'库存',key:'inventory'},
                    {name:'核算',key:'AdAccount'},
                    {name:'财务',key:'finance;'},
                    {name:'凭证接口',key:'CredentialInterface'},
                    {name:'零售/会员',key:'retailVip'},
                    {name:'生产/委外',key:'productOutsource'},
                ]
            }
        },
        methods:{
            submit(name){
                //提交弹窗的改动
                console.log('submit')
                this.$refs[name].style.display = 'none';
            },
            //跳转选项卡
            switchTab(key,index){
                this.switchIndex = index
            },
            showBlock(name){
                console.log(name)
                this.$refs[name].style.display = 'flex';
            },
            close(name){
                this.$refs[name].style.display = 'none'
            }
        }
    }
</script>

<style scoped lang="scss">
    .systemManage{
        width:60%;
        height:465px;
        margin: 0 auto;
        display:flex;
        justify-content: space-around;
        align-items: center;
        flex-wrap:wrap;
        .itemBlock{
            position: relative;
            display:inline-block;
            width: 70px;
            height:60px;
            // border:1px solid red;
            display:flex;
            flex-direction:column;
            justify-content: space-around;
            align-items: center;
            margin-right:40px;
            cursor: pointer;
            .iconfontItem{
                font-size:30px;
                color:#0094e7;
                cursor: pointer;
            }
            .blockName{
                font-size:12px;
                cursor: pointer;
            }
        }
        .itemBlock:hover {
            .blockName{
                font-size:14px;
                color:rgb(115, 152, 167);
            }
            .iconfontItem{
                font-size:35px;
                color:rgb(115, 152, 167);
            }   
        }
    }
    .optionSetting{
        position:fixed;
        top:0;
        right:0;
        bottom:0;
        left:0;
        background:rgba(0,0,0,0.5);
        z-index:9999999;
        display:flex;
        justify-content: center;
        align-items: center;
        display:none;
        min-width:780px;
        .settingContent{
            width:780px;
            height:678px;
            background:white;
            border-radius:5px;
            display:flex;
            flex-direction: column;
            .fixedTitle{
                width:100%;
                height:45px;
                border-bottom:1px solid lightgray;
                display:flex;
                justify-content: space-between;
                align-items: center;
                padding:0 10px;
                box-sizing: border-box;
                h2{
                    font-size:18px;
                    font-weight:normal;
                }
                .iconfont{
                    font-size:24px;
                    margin-left:10px;
                    color:rgb(100,100,100);
                    cursor: pointer;
                }
                .iconfont:hover{
                    color:#0094e7;
                }
            }
            .blockContentPadding{
                width:100%;
                flex:1;
                padding:15px;
                box-sizing: border-box;
                .blockContent{
                    width:100%;
                    height:100%;
                    background:#fafdfd;
                    padding:3px 0 5px 5px;
                    box-sizing: border-box;
                    
                    .switchBtnGroup{
                        width:100%;
                        display:flex;
                        align-items: baseline;
                        .btnItem{
                            height:20px;
                            width:67px;
                            border:1px solid rgb(200,200,200);
                            border-top-left-radius:3px;
                            border-top-right-radius:3px;
                            background:linear-gradient(to bottom,rgb(250,250,250),rgb(240,240,240));
                            cursor: pointer;
                            outline: none;
                            color:#444444;
                        }
                    }
                    // 选项卡下的第一个框
                    .switchContent{
                        width:100%;
                        height:360px;
                        border-left:1px solid lightgray; 
                        border-bottom:1px solid lightgray; 
                        padding:5px 8px;
                        overflow: auto;
                        box-sizing: border-box;
                        .overflowBox{
                            display:flex;
                            flex-wrap:wrap;
                            // height:40px;
                            .publicData,.dataPrecision,.barcodeManagement,.batchManagement{
                                width:300px;
                                // float:left;
                                margin-bottom:5px;
                                fieldset{
                                    padding:5px;
                                    box-sizing: border-box;
                                }
                                .lineDivi{
                                    width:100%;
                                    display:flex;
                                    align-items: center;
                                    justify-content: space-evenly;
                                    .spanItem{
                                        width:100%;
                                        text-align:left;
                                    }
                                    .labelItem{
                                        display:flex;
                                        height:23px;
                                        align-items: center;
                                        input{
                                            margin-right:5px;
                                        }
                                        span{
                                            width:200px;
                                        }
                                    }
                                    .radiogroup{
                                        width:100%;
                                        display:flex;
                                        justify-content: space-evenly;
                                        align-items: center;
                                    }
                                    label.labelItem{
                                        width:100%;
                                        text-align: left;
                                    }
                                    .singleItem{
                                        padding:05px;
                                        box-sizing: border-box;
                                    }
                                }
                                .controlItemBox{
                                    width:80%;
                                }
                            }
                            .dataPrecision{
                                width:395px;
                                margin-left:14px;
                                .selectItem{
                                    width:100px;
                                }
                                .seizeSeat{
                                    width:100px;
                                }
                                .labelItem{
                                    //右边给180
                                    width:150px;
                                    .spanTitle{
                                        margin-right:5px;
                                        text-align:right;
                                    }
                                }
                                .TurnZero{
                                    width:95px !important;
                                    margin-left:3px;
                                }
                                .rightLabelItem{
                                    width:200px;
                                }
                            }
                            .batchManagement{
                                width:394px;
                                margin-left:13px;
                                margin-top:-15px;
                                box-sizing: border-box;
                                .ruleBlock{
                                    border:1px solid gray;
                                    width:100%;margin-top:5px;
                                    height:185px;
                                    // overflow:scroll;
                                    position:relative
                                }
                                .tableContent{
                                    position: absolute;
                                    top:20px;
                                    right:0;
                                    bottom:0;
                                    left:0;
                                    border:1px solid red;
                                    overflow:scroll;
                                }
                                .autoProduceBatchNumberTableTitle{
                                    width:256px;
                                    height:25px;
                                    line-height:25px;
                                    background:rgb(186, 219, 241);
                                    border:1px solid red;
                                    margin-left:7px;
                                    border:1px solid gray;
                                    border-bottom:none;
                                    box-sizing: border-box;
                                    span{
                                        display:inline-block;
                                        text-align:center;
                                        border-right:1px solid gray;
                                        box-sizing: border-box;
                                    }
                                    span:last-of-type{
                                        border:none;
                                    }
                                }
                                .autoProduceBatchNumberTable{
                                    width:256px;
                                    margin:0 7px;
                                    text-align:center;
                                    // tr:first-of-type{
                                    //     height:25px;
                                    //     line-height:25px;
                                    //     cursor:auto ;
                                    //     background:rgb(190, 209, 233);
                                    // }
                                    tr.contentItem:hover{
                                        background:rgb(247, 233, 157);
                                    }
                                    th{
                                        border:1px solid gray;
                                    }
                                    td{
                                        .length{
                                            height:23px;
                                            border:none;
                                            width:100%;
                                            background:none;
                                            outline: none;
                                        }
                                        border:1px solid gray;
                                        box-sizing: border-box;
                                        
                                    }
                                    
                                }
                            }
                        }
                    }
                    // 选项卡被选中时候的样式
                    .linkSwitch{
                        height:23px !important;
                        border-bottom:none !important;
                        box-sizing: border-box !important;
                        border-radius: 0 !important;
                    }
                    .explainWords{
                        width:99%;
                        height:130px;
                        border:1px solid lightgray;
                        box-sizing: border-box;
                    }
                    .opBtnGroup{
                        width:99%;
                        height:60px;
                        display:flex;
                        justify-content: flex-end;
                        align-items: center;
                        
                        button{
                            width:85px;
                            height:30px;
                            font-size:14px;
                            background:none;
                            border:1px solid gray;
                            margin-left:10px;
                            border-radius:5px;
                            background:white;
                            outline: none;
                            cursor: pointer;
                        }
                    }
                }
            }
        }
    }
</style>