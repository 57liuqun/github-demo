<template>
  <div class="editCell">
    <el-table :data="dataSource" border  size="mini" :highlight-current-row="table1HighLight1"  
        :row-class-name="showHighLight"
        @row-click='rowClickFn' 
        @current-change="currentChangeFn" 
        ref="dataSource"
        style="width:600px;margin:30px auto;"
    >
        <el-table-column label="姓名"  align="center" style="width:100px"> 
            <template slot-scope="scope">
                <el-input  v-model="scope.row.name" size="mini" style="width:150px"  v-if="scope.row.isEditName" @blur="setIsEditName('1',scope.row)" ref="isEditNameNode" focus>
                    <i slot="append" class="el-icon-search"></i>
                </el-input>
                <span v-else @click="setIsEditName('0',scope.row)">{{scope.row.name}}</span>
            </template>
        </el-table-column>
        <el-table-column label="年龄" prop="age" align="center"> 
            <template slot-scope="scope" >
                <el-input  v-model="scope.row.age" size="mini" style="width:150px" v-if="scope.row.isEditAge" @blur="setIsEditAge('1',scope.row)" ref="isEditAgeNode" focus>
                    <i slot="append" class="el-icon-search"></i>
                </el-input>
                <span v-else @click="setIsEditAge('0',scope.row)">{{scope.row.age}}</span>
            </template>
        </el-table-column>
        <el-table-column label="备注" prop="remark" align="center"> 
            <template slot-scope="scope" size="mini">
                <el-input  v-model="scope.row.remark"  size="mini" style="width:150px" v-if="scope.row.isEditRemark" @blur="setIsEditRemark('1',scope.row)" ref="isEditRemarkNode" focus>
                    <i slot="append" class="el-icon-search"></i>
                </el-input>
                <span v-else @click="setIsEditRemark('0',scope.row)">{{scope.row.remark}}</span>
            </template>
        </el-table-column>
    </el-table>    
    <div style="height:30px"></div>
    <el-table :data="dataSource" border fit size="mini" :highlight-current-row="table1HighLight2" 
        :row-class-name="showHighLight"
        @row-click='rowClickFn' 
        @current-change="currentChangeFn" 
        ref="dataSource2"
    >
        <el-table-column label="姓名" prop="name" align="center"> 

        </el-table-column>
        <el-table-column label="年龄" prop="age" align="center"> 

        </el-table-column>
        <el-table-column label="备注" prop="remark" align="center"> 

        </el-table-column>
    </el-table>
    <input v-focus/>
  </div>
</template>
<script>
export default {
  data:function(){
    return {
      dataSource:[
          {name:'name2121',age:'23',remark:'sdfbsjbe239u'},
          {name:'name2343',age:'21',remark:'sdfbsjbe239u'},
          {name:'name5234',age:'22',remark:'sdfbsjbe239u'},
          {name:'name5345',age:'25',remark:'sdfbsjbe239u'},
      ],
      dataSource2:[
          {name:'name2121',age:'23',remark:'sdfbsjbe239u'},
          {name:'name2343',age:'21',remark:'sdfbsjbe239u'},
          {name:'name5234',age:'22',remark:'sdfbsjbe239u'},
          {name:'name5345',age:'25',remark:'sdfbsjbe239u'},
      ],
      table1HighLight1:true,
      table1HighLight2:false,
      table1HighLight3:false,
      table1HighLight4:false,
      table1HighLight5:false,
      preHighLight:'',//记录上一个高亮的名字
      indexTb1:0,
      indexTb2:0,
    }
  },
  mounted(){
      this.setHighLightrowFirst('dataSource',this.dataSource[0],this.indexTb1)
      window.addEventListener('keydown',()=>{
        this.showKeyCode()
      })
  },
  directives:{
    focus:{
      inserted(el){
        el.focus()
      }
    }
  },
  methods:{
    setIsEditName(code,row){
        console.log(code)
        //isEditRemark
        debugger
        if(code == 0){
            this.dataSource[row.index].isEditName = true
        }else{
            this.dataSource[row.index].isEditName = false
            this.$refs.isEditNameNode.focus()
        }
        
        this.dataSource = JSON.parse(JSON.stringify(this.dataSource))
        console.log(this.dataSource)
    },
    setIsEditAge(code,row){
        //isEditAge
        if(code == 0){
            this.dataSource[row.index].isEditAge = true
        }else{
            this.$refs.isEditAgeNode.focus()
            this.dataSource[row.index].isEditAge = false
        }
        this.dataSource = JSON.parse(JSON.stringify(this.dataSource))
        console.log(this.dataSource)
    },
    setIsEditRemark(code,row){
        //isEditRemark
        if(code == 0){
            this.dataSource[row.index].isEditRemark = true
        }else{
            this.$refs.isEditAgeNode.focus()
            this.dataSource[row.index].isEditRemark = false
        }
        this.dataSource = JSON.parse(JSON.stringify(this.dataSource))
        console.log(this.dataSource)
    },

    setHighLightrowFirst(node,row,dataIndex){
        this.dataIndex = 0
        this.$refs[node].setCurrentRow(this[node][dataIndex]);
    },
    showKeyCode(){
        // document.onkeydown=function(event){
        //     console.log(event)
        //     console.log(`你按了${event.code}键`)
        // }
        //阻止默认的拉动滚动条的事件
        document.onkeydown=function(e){
            var e = e || window.event;
            if(e.keyCode == '38' || e.keyCode == '40'){
                if(e.preventDefault){
                    e.preventDefault();
                    
                }else{
                    e.returnValue = false;
                }
            }
        }
        console.log(event.keyCode)
        // debugger
        if(event.keyCode == 40){
            //向下的按键
            if(this.table1HighLight1){
                if(this.indexTb1 <this.dataSource.length -1){
                    this.indexTb1 = this.indexTb1 + 1
                    this.$refs.dataSource.setCurrentRow(this.dataSource[this.indexTb1])
                }
            }else if(this.table1HighLight2){
                if(this.indexTb2 <this.dataSource2.length -1){
                    this.indexTb2 = this.indexTb2 + 1
                    this.$refs.dataSource2.setCurrentRow(this.dataSource2[this.indexTb2])
                }
            }
        }else if(event.keyCode == 38){
            //向上的按键
            if(this.table1HighLight1){
                if(this.indexTb1 > 0){
                    this.indexTb1 = this.indexTb1 - 1
                    this.$refs.dataSource.setCurrentRow(this.dataSource[this.indexTb1])
                }
            }else if(this.table1HighLight2){
                if(this.indexTb2 > 0){
                    this.indexTb2 = this.indexTb2 - 1
                    this.$refs.dataSource2.setCurrentRow(this.dataSource2[this.indexTb2])
                }
            }
        }
    },
    showHighLight(row,index){
        //:row-class-name 添加index
        row.row.index = row.rowIndex
    },
    rowClickFn(row,event,column){
        // console.log(row)
        // this.dataSource[row.index].isEdit = !this.dataSource[row.index].isEdit
        // this.dataSource = JSON.parse(JSON.stringify(this.dataSource))
        // console.log(this.dataSource)
    },
    currentChangeFn(val,oldVal){
        // console.log(val)
    }
  }
}
</script>>
<style>

</style>

