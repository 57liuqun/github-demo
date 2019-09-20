<template>
    <div class="miniateTansfer">
        <div class="leftBlock">
            <div class="blockTitle">
                <label><input type="checkbox" v-model="leftChooseAll" @change="selectLeftAll" :disabled="leftList.length == 0" />列表1</label>
                <span class="showNum">{{leftChecked.length}}/{{leftList.length}}</span>
            </div>
            <div class="searchBar">
                <!-- 搜索条 -->
                <input placeholder="输入关键词" v-model="keyWordLeft" @input="searchLeftList"/>
                <i class="el-icon-search" @click="searchLeftList"></i>
            </div>
            <div class="blockContent">
                <label v-for="(item,index) in leftList" :key="index" class><input type="checkbox" v-model="item.checked" @change="selectLeftItem(index)"/><span>{{item.label}}</span></label>
                <div class="showNoData" v-if="leftList.length == 0">暂无数据</div>
            </div>
        </div>
        <div class="middleBtn">
            <button class="btnItem" :disabled="rightChecked.length == 0" :class="rightChecked.length > 0 ? 'canTransfer' : 'disabled' "  @click="rightToLeft" >&lt;</button>
            <button class="btnItem" :disabled="leftChecked.length == 0" :class="leftChecked.length > 0 ? 'canTransfer' : 'disabled' "  @click="leftToRight" >&gt;</button>
        </div>
        <div class="rightBlock">
            <div class="blockTitle">
                <label><input type="checkbox" v-model="rightChooseAll" @change="selectRightAll" :disabled="rightList.length == 0"/>列表2</label>
                <span class="showNum" >{{rightChecked.length}}/{{rightList.length}}</span>
            </div>
            <div class="searchBar">
                <!-- 搜索条 -->
                <input placeholder="输入关键词" v-model="keyWordRight" @input="searchRightList"/>
                <i class="el-icon-search" @click="searchRightList" ></i>
            </div>
            <div class="blockContent">
                <label v-for="(item,index) in rightList" :key="index" ><input type="checkbox" v-model="item.checked" @change="selectRightItem(index)" /><span>{{item.label}}</span></label>
                <div class="showNoData" v-if="rightList.length == 0">暂无数据</div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data:function(){
        const leftList = [
                {label:'one',id:'1',checked:false},
                {label:'two',id:'2',checked:false},
                {label:'three',id:'3',checked:false},
                {label:'four',id:'4',checked:false},
                {label:'five',id:'5',checked:false},
                {label:'six',id:'6',checked:false},
                {label:'seven',id:'7',checked:false}
            ]
        const rightList = [
                {label:'eight',id:'8',checked:false},
                {label:'nine',id:'9',checked:false},
                {label:'ten',id:'10',checked:false},
                {label:'eleven',id:'11',checked:false},
                {label:'twlfy',id:'12',checked:false},
                {label:'thirteen',id:'13',checked:false},
                {label:'forteen',id:'14',checked:false}
            ]
        return {
            leftChooseAll:false,
            rightChooseAll:false,
            leftChecked:[],//存的id
            leftListChecked :[],//存的对象
            leftListUnchecked:JSON.parse(JSON.stringify(leftList)),//未选择的
            rightListChecked:[],
            rightChecked:[],
            rightListUnchecked:JSON.parse(JSON.stringify(rightList)),
            leftList:leftList,
            rightList:rightList,
            keyWordLeft:'',
            keyWordRight:'',
            leftListSave:[],//在搜索的时候存储原数组
            rightListSave:[]
        }
    },
    mounted(){
       this.leftListSave = JSON.parse(JSON.stringify(this.leftList))
       this.rightListSave = JSON.parse(JSON.stringify(this.rightList))
    },
    methods:{
        //简单对象list自定义排序
        /**  objList.sort((a,b)=>{
            return a.id - b.id
        })
        */

        // 搜索
        searchLeftList(){
            let arr = []
            console.log(this.keyWordLeft)
            if( this.keyWordLeft !== ''){
                for(let i = 0;i<this.leftList.length;i++){
                // 模糊查询
                console.log(this.leftList[i].label)
                    if(this.leftList[i].label.toLowerCase().match(this.keyWordLeft.toLowerCase()) !== null){
                        arr.push(this.leftList[i])
                    }
                }
                //按照id从小到大排序
                this.leftList = arr.sort((a,b)=>{
                    return a.id - b.id
                })
            }else{
                console.log(this.leftListSave)
                this.leftList = JSON.parse(JSON.stringify(this.leftListSave)).sort((a,b)=>{
                    return a.id - b.id
                })
            }
        },
        searchRightList(){
            let arr = []
            if( this.keyWordRight !== ''){
                for(let i = 0;i<this.rightList.length;i++){
                // 精确查询
                    if(this.rightList[i].label.toLowerCase().match(this.keyWordRight.toLowerCase())!== null ){
                        arr.push(this.rightList[i])
                    }
                }
                this.rightList = arr.sort((a,b)=>{
                    return a.id - b.id
                })
            }else{
                this.rightList = JSON.parse(JSON.stringify(this.rightListSave)).sort((a,b)=>{
                    return a.id - b.id
                })
            }
        },
        rightToLeft(){
            this.rightList = JSON.parse(JSON.stringify(this.rightListUnchecked))
            this.rightListSave = JSON.parse(JSON.stringify(this.rightList))
            // 将选中状态取消
            for(let j = 0;j<this.rightListChecked.length;j++){
                this.rightListChecked[j].checked = false
            }
            this.leftList = this.leftList.concat(JSON.parse(JSON.stringify(this.rightListChecked))).sort((a,b)=>{
                return a.id - b.id
            })
            this.leftListSave = JSON.parse(JSON.stringify(this.leftList)).sort((a,b)=>{
                return a.id - b.id
            })
            console.log("保存数组00")
            console.log(this.leftListSave)
            // 将未选中重新赋值
            this.leftListUnchecked = (()=>{
                // 不能直接等于,会有还是选中的值的情况
                let arr = []
                for(let k = 0 ;k<JSON.parse(JSON.stringify(this.leftList)).length;k++){
                    if(!JSON.parse(JSON.stringify(this.leftList))[k].checked){
                        //
                        arr.push(JSON.parse(JSON.stringify(this.leftList))[k])
                    }
                }
                return arr
            })();
            this.rightListChecked = [];//置空

            this.rightChooseAll = false
            for(let i = 0;i<this.rightListChecked.length;i++){
                this.rightListChecked[i].checked = false
            }
            this.rightChecked = []
            // //
            // console.log('右边向左边转移数据之后,左边数据的情况')
            // console.log(this.leftList)
            // console.log(this.leftListChecked)
            // console.log(this.leftListUnchecked)
            // console.log('右边自身的数据')
            // console.log(this.rightList)
            // console.log(this.rightListChecked)
            // console.log(this.rightListUnchecked)
        },
        leftToRight(){
            this.leftList = JSON.parse(JSON.stringify(this.leftListUnchecked))
            this.leftListSave = JSON.parse(JSON.stringify(this.leftList))
            // 将选中状态取消
            for(let j = 0;j<this.leftListChecked.length;j++){
                this.leftListChecked[j].checked = false
            }
            this.rightList = this.rightList.concat(JSON.parse(JSON.stringify(this.leftListChecked))).sort((a,b)=>{
                return a.id - b.id
            })
            this.rightListSave = JSON.parse(JSON.stringify(this.rightList)).sort((a,b)=>{
                return a.id - b.id
            })
            // 将未选中重新赋值
            this.rightListUnchecked = (()=>{
                // JSON.parse(JSON.stringify(this.rightList))
                let arr = []
                for(let k = 0 ;k<JSON.parse(JSON.stringify(this.rightList)).length;k++){
                    if(!JSON.parse(JSON.stringify(this.rightList))[k].checked){
                        //
                        arr.push(JSON.parse(JSON.stringify(this.rightList))[k])
                    }
                }
                return arr
            })();
            this.leftListChecked = [];//置空

            this.leftChooseAll = false
            this.leftChecked = []
            for(let i = 0;i<this.leftListChecked.length;i++){
                this.leftListChecked[i].checked = false
            }
            // console.log("左向右转移数据之后右边的数据情况")
            // console.log(this.rightList)
            // console.log(this.rightListChecked)
            // console.log(this.rightListUnchecked)
            // console.log('左边自身的数据')
            // console.log(this.leftList)
            // console.log(this.leftListChecked)
            // console.log(this.leftListUnchecked)
        },
        selectLeftAll(){
            if(this.leftChooseAll){
                console.log(1)
                this.leftChecked = (()=>{
                    let array = []
                    for(let i = 0 ;i<this.leftList.length;i++){
                        array.push(this.leftList[i].id)
                    }
                    return array
                })()
                this.leftListChecked = JSON.parse(JSON.stringify(this.leftList))
                this.leftListUnchecked = []
                for(let i = 0 ;i<this.leftList.length;i++){
                    this.leftList[i].checked = true
                }
            }else{
                console.log(2)
                this.leftChecked = []
                 for(let i = 0 ;i<this.leftList.length;i++){
                    this.leftList[i].checked = false
                }
                this.leftListChecked = []
                this.leftListUnchecked = JSON.parse(JSON.stringify(this.leftList))
                console.log("此时的leftListUnchecked")
                console.log( this.leftListUnchecked )
            }
            console.log(this.leftChecked)
        },
        selectRightAll(){
            if(this.rightChooseAll){
                console.log(1)
                this.rightChecked = (()=>{
                    let array = []
                    for(let i = 0 ;i<this.rightList.length;i++){
                        array.push(this.rightList[i].id)
                    }
                    return array
                })()
                this.rightListChecked = JSON.parse(JSON.stringify(this.rightList))
                this.rightListUnchecked = []

                for(let i = 0 ;i<this.rightList.length;i++){
                    this.rightList[i].checked = true
                }
            }else{
                console.log(2)
                this.rightChecked = []
                 for(let i = 0 ;i<this.rightList.length;i++){
                    this.rightList[i].checked = false
                }
                this.rightListChecked = []
                this.rightListUnchecked = JSON.parse(JSON.stringify(this.rightList))
            }
        },
        selectLeftItem(index){
            let indexUn = 0
            let index22= this.leftChecked.indexOf(this.leftList[index].id)
            if(index22 !== -1){
                this.leftChecked.splice(index22,1)
                this.leftListChecked.splice(index22,1)
                this.leftListUnchecked.push(this.leftList[index])
            }else{
                this.leftChecked.push(this.leftList[index].id)
                this.leftListChecked.push(this.leftList[index])
                for(let j = 0;j<this.leftListUnchecked.length;j++){
                    if(this.leftListUnchecked[j].id == this.leftList[index].id ){
                        indexUn = j;
                        break;
                    }
                }
                console.log(this.leftListUnchecked[indexUn])
                this.leftListUnchecked.splice(indexUn,1);//截去未选数组

            }
            if(this.leftChecked.length == this.leftList.length ){
                this.leftChooseAll = true
            }else{
                this.leftChooseAll = false
            }
            console.log("this.leftListChecked")
            console.log(this.leftListChecked)
            console.log("this.leftListUnChecked")
            console.log(this.leftListUnchecked)
        },
        selectRightItem(index){
            let indexUn = 0
            let index22= this.rightChecked.indexOf(this.rightList[index].id)
            if(index22 !== -1){
                this.rightChecked.splice(index22,1)
                this.rightListChecked.splice(index22,1)
                this.rightListUnchecked.push(this.rightList[index])
            }else{
                this.rightChecked.push(this.rightList[index].id)
                this.rightListChecked.push(this.rightList[index])
                for(let j = 0;j<this.rightListUnchecked.length;j++){
                    if(this.rightListUnchecked[j].id == this.rightList[index].id ){
                        indexUn = j;
                        break;
                    }
                }
                this.rightListUnchecked.splice(indexUn,1);//截去未选数组
            }
            if(this.rightChecked.length == this.rightList.length && this.rightList.length > 0){
                this.rightChooseAll = true
            }else{
                this.rightChooseAll = false
            }
            console.log("this.rightListChecked")
            console.log(this.rightListChecked)
            console.log("this.rightListUnChecked")
            console.log(this.rightListUnchecked)
        }
    }
}
</script>
<style scoped>
    .miniateTansfer{
        display:flex;
        align-items: center;
        width:350px;
        margin:100px auto;
    }
    .leftBlock,.rightBlock{
        display:flex;
        flex-direction: column;
        align-items: center;
        border:1px solid gray;
        width:150px;
    }
    .leftBlock .blockTitle,.rightBlock .blockTitle{
        width:100%;
        display:flex;
        justify-content: space-between;
        align-items: center;
        padding:0 10px;
        box-sizing: border-box;
        background:#e8e8e8;
        height:40px;
    }
    .leftBlock .blockTitle .showNum,.rightBlock .blockTitle .showNum{
        color:gray;
    }
    .leftBlock .searchBar,
    .rightBlock .searchBar{
        width:95%;
        height:30px;
        display:flex;
        align-items: center;
        border:1px solid lightgrey;
        margin:10px auto;
        padding:0 5px;
        box-sizing: border-box;
        border-radius:20px;
    }
    .leftBlock .searchBar input,
    .rightBlock .searchBar input{
        width:85%;
        margin:0 5px;
        height:100%;
        box-sizing: border-box;
        border-radius:5px;
        border: none;
        outline: none;

    }
    .leftBlock .searchBar .el-icon-search,
    .rightBlock .searchBar .el-icon-search{
        cursor: pointer;
    }
    .leftBlock .searchBar .el-icon-search:hover,
    .rightBlock .searchBar .el-icon-search:hover{
        color:#0094e7;
    }
    /* el-icon-search */
    .leftBlock .blockContent,
    .rightBlock .blockContent{
        width:100%;
        height:200px;
        overflow-y: auto;
        display:flex;
        flex-direction: column;
        align-items: center;
        padding:0 10px;
        box-sizing: border-box;
    }
    .leftBlock .blockContent label,
    .rightBlock .blockContent label{
        width:100%;
        height:30px;
        line-height:30px;
    }
    .leftBlock .blockContent .showNoData,
    .rightBlock .blockContent .showNoData{
        height:100%;
        display:flex;
        justify-content: center;
        align-items: center;
    }
    /* showNoData */
    .middleBtn{
        display:flex;
        flex-direction: column;
        align-items: center;
        margin:0 10px;
    }
    .middleBtn button{
        display:inline-block;
        width:40px;
        height:40px;
        background:#e8e8e8;
        border-radius:50%;
        text-align: center;
        line-height:40px;
        color:gray;
        margin-bottom:10px;
        border:1px solid gray;
        outline:none;
        cursor: pointer;
    }
    .canTransfer{
        color:white !important;
        background:#0094e7 !important;
        border-color:#0094e7 !important;
    }
    .disabled{
        cursor:not-allowed;
    }
</style>