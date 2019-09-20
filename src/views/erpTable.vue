<template>
  <div class="inventory" @click="inventoryClick">
    <div class="inventory_left">
      <div class="inventory_left_top">
        <input type="text" name="" v-model="setPostionVal" id="">
        <button @click="findPosition">定位</button>
      </div>
      <div class="inventory_left_tree_cont">
        <el-tree :data="data" :props="defaultProps" show-checkbox ref="tree" node-key='label' highlight-current accordion @node-click="handleNodeClick"></el-tree>
      </div>
    </div>
    <div class="inventory_right">
      <div class="inventory_right_tools">
        <el-button class="inventory_right_tools_btn" @click="addVisible = true" type="text">新增</el-button>
        <el-button class="inventory_right_tools_btn" type="text">修改</el-button>
        <el-button class="inventory_right_tools_btn" @click="deleteTb" type="text">删除</el-button>
        <el-button class="inventory_right_tools_btn" type="text">复制</el-button>
        <el-button class="inventory_right_tools_btn" type="text">查找</el-button>
        <el-button class="inventory_right_tools_btn" @click="tbColumnVisible = true" type="text">栏目</el-button>
        <el-dropdown placement="bottom-start">
          <span class="el-dropdown-link">
            <el-button class="inventory_right_tools_btn" style="margin-right:-5px" type="text">打印</el-button><i class="el-icon-arrow-down el-icon--right"></i>
          </span>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item>打印</el-dropdown-item>
            <el-dropdown-item>预览</el-dropdown-item>
            <el-dropdown-item>标签模板</el-dropdown-item>
            <el-dropdown-item>标签打印</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
        <el-dropdown placement="bottom-start">
          <span class="el-dropdown-link">
            <el-button class="inventory_right_tools_btn" style="margin-right:-5px" type="text">导入</el-button><i class="el-icon-arrow-down el-icon--right"></i>
          </span>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item>下载分类模板</el-dropdown-item>
            <el-dropdown-item>导入分类</el-dropdown-item>
            <el-dropdown-item>下载存货模板</el-dropdown-item>
            <el-dropdown-item>导入存货</el-dropdown-item>
            <el-dropdown-item>下载存货列导入模板</el-dropdown-item>
            <el-dropdown-item>存货列导入</el-dropdown-item>
            <el-dropdown-item>导入图片</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
        <el-button class="inventory_right_tools_btn" type="text">导出</el-button>
      </div>
      <el-table :data="tableData" highlight-current-row :header-row-style="headerStyle" @current-change="handleCurrentChange" size='small' style="width: 100%" border height="calc(100% - 70px)">
        <el-table-column type="index" label="序号" fixed="left" width="50" align="center"></el-table-column>
        <template v-for="(v,k) in tableColumn">
          <el-table-column :show-overflow-tooltip="true" :resizable="false" v-if="v.isShow" :key="k" header-align="center" :label="v.label" :min-width="v.width" :width="v.width">
            <template slot-scope="scope">
              <p class="table_cont_p" @click="clickRow(scope.$index)">{{scope.row[v.prop]}}</p>
            </template>
          </el-table-column>
        </template>
      </el-table>
      <div class="pagination_line">
        <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="pageRequest.pageNum" :page-sizes="[100, 200, 300, 400]" :page-size="pageRequest.pageSize" layout="total, sizes, prev, pager, next, jumper" :total="400"></el-pagination>
      </div>
    </div>
    <!-- 表头设置弹框 -->
    <el-dialog title="列显示" :close-on-click-modal="false" :visible.sync="tbColumnVisible" width="30%">
      <div class="set_tab_box">
        <el-table :data="tableColumnBase" border>
          <el-table-column type="index" width="50" align="center"></el-table-column>
          <el-table-column label="字段名称" prop="label" align="center"></el-table-column>
          <el-table-column label="显示名称" prop="label" align="center"></el-table-column>
          <el-table-column label="显示" align="center">
            <template slot-scope='scope'>
              <el-checkbox v-model="scope.row.isShow"></el-checkbox>
            </template>
          </el-table-column>
        </el-table>
      </div>
      <span slot="footer" class="dialog-footer">
        <el-button size="mini" @click.stop="tbColumnVisible = false">取 消</el-button>
        <el-button size="mini" type="primary" @click.stop="confirmTbColumnVisible">确 定</el-button>
      </span>
    </el-dialog>
    <!-- 新增弹窗 -->
    <el-dialog title="新增存货" :close-on-click-modal="false" :visible.sync="addVisible" width="1000px">
      <div class="set_tab_box">
        <el-form :inline="true" :model="dialogForm" size="mini" label-width="130px" class="demo-form-inline">
          <el-form-item label="存货编码">
            <el-input v-model="dialogForm.valu8"></el-input>
          </el-form-item>
          <el-form-item label="存货名称">
            <el-input v-model="dialogForm.valu81" style="width:251px"></el-input>
          </el-form-item>
          <el-form-item label="助记码">
            <el-input v-model="dialogForm.valu82"></el-input>
          </el-form-item>
          <el-form-item label="规格型号" >
            <el-input v-model="dialogForm.valu3" style="width:251px"></el-input>
          </el-form-item>
          <el-form-item label="计价方式">
            <el-select v-model="dialogForm.value" style="width:175px" placeholder="请选择">
              <el-option v-for="(item,index) in pricingMode" :key="index" :label="item.title" :value="item.title"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="所属类别" width="300px">
            <div class="input_select" @click.stop="stopPop">
              <!-- value,tableNode,indexData,currentRow -->
              <input v-model="dialogForm.value6" class="input_select_input" 
              ref="sslb" 
              @focus="showSelectFocus('showSelectPm','tableAttrCato','indexSSFL','ssflCurrentRow')"
              @input="filterTableList('value6','tableAttrCato','oldTableAttrCato','indexSSFL',['flbm','flmc'])" 
              @keydown.38="inputBlurChooseTableUp('tableAttrCato','indexSSFL','ssflCurrentRow')"  
              @keydown.40="inputBlurChooseTableDown('tableAttrCato','indexSSFL','ssflCurrentRow')" 
              @blur="inputSelectBlur" 
              @keyup.enter="inputEnter('showSelectPm','value6','ssflCurrentRow','flbm','sslb')" />
              <span class="input_select_select" @click.stop="showSelect('showSelectPm')">
              <i class="el-icon-arrow-down"></i>
              </span>
              <!-- 下拉的部分 -->
              <!-- <div class="selectBlock pricingMode" v-if="showSelectPm">
              <span v-for="(item,index) in pricingMode" :key="index" :title="item.title" @click.stop="select('showSelectPm','value6',item.title)" >{{item.title}}</span>
              </div> -->
              <div class="selectBlock pricingMode" v-if="showSelectPm" style="top:33px">
                <el-table :data="tableAttrCato" border size="mini"   style="width:251px;" @row-click='tbRowClickSSFL' 
                @current-change="currentSelectSSFL"  
                max-height="150" 
                :header-row-style="headerStyle22" 
                :fit="true" 
                highlight-current-row 
                ref="tableAttrCato" 
                :row-class-name="tableRowClassName"
                >
                  <el-table-column label="分类编码" prop="flbm" align="center" ></el-table-column>
                  <el-table-column label="分类名称" prop="flmc" align="center"></el-table-column>
                  <el-table-column label="详细" align="center">
                    <template slot-scope='scope'>
                      <!-- <el-checkbox v-model="scope.row.isShow"></el-checkbox> -->
                      <span @click.stop="findDetail(scope.row)" style="color:#409EFF;cursor:pointer">详细</span>
                    </template>
                  </el-table-column>
                </el-table>
                <div class="btnGroup">
                  <span class="add">增加</span>
                  <span class="listBoard">栏目</span>
                  <span class="all">全部</span>
                </div>
              </div>
            </div>
          </el-form-item>

          <el-tabs style="height: 200px;">
            <el-tab-pane label="基本信息">
              <div class="tabs_one_box_title">
                <span>基本信息</span>
                <div></div>
              </div>
              <div class="tabs_one_box_cont">
                <el-form-item label="税率">
                  <el-select v-model="dialogForm.value8" style="width:175px" placeholder="请选择">
                    <el-option label="17" value="17"></el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="品牌">
                  <el-select v-model="dialogForm.value9" style="width:175px" placeholder="请选择">
                    <el-option label="潮牌" value="潮牌"></el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="金税税收分类编码">
                  <el-input v-model="dialogForm.valu89568"></el-input>
                </el-form-item>
                <el-form-item label="新品周期">
                  <el-input v-model="dialogForm.valu898"></el-input>
                </el-form-item>
                <el-form-item label="建档日期">
                  <el-date-picker v-model="dialogForm.value1333" style="width:175px" type="date" placeholder="建档日期"></el-date-picker>
                </el-form-item>
                <el-form-item label="  ">
                  <el-checkbox v-model="dialogForm.valu8981">新品</el-checkbox>
                  <el-checkbox v-model="dialogForm.valu89812">停用</el-checkbox>
                </el-form-item>
              </div>
              <div class="tabs_one_box_title">
                <span>计量单位</span>
                <div></div>
              </div>
              <div class="tabs_one_box_cont">
                <el-form-item label="计量方式">
                  <el-select v-model="dialogForm.value67" class="metering_drop-down_box" placeholder="请选择">
                    <el-option label="单计量" value="single"></el-option>
                    <el-option label="多计量" value="many"></el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="计量单位" >
                <!-- <el-select v-model="dialogForm.value6" style="width:175px" placeholder="请选择">
                  <el-option label="移动平均" value="移动平均"></el-option>
                </el-select> -->
                <div class="input_select" @click.stop="stopPop">
                  <!-- showSelectFocus 可作为公用方法，只要传递data待操作的data值就可以 -->
                  <input v-model="dialogForm.value7" 
                  class="input_select_input" 
                  ref="jldw"  
                  @focus="showSelectFocus('showSelectUnit','tableUnit','indexJLDW','jldwCurrentRow')"
                  @input="filterTableList('value7','tableUnit','oldTableUnit','indexJLDW',['code','unit'])" 
                  @keydown.38="inputBlurChooseTableUp('tableUnit','indexJLDW','jldwCurrentRow')"  
                  @keydown.40="inputBlurChooseTableDown('tableUnit','indexJLDW','jldwCurrentRow')" 
                  @keyup.enter="inputEnter('showSelectUnit','value7','jldwCurrentRow','unit','jldw')"
                  /> <!-- -->
                  <span class="input_select_select" @click.stop="showSelect('showSelectPm')">
                  <i class="el-icon-arrow-down"></i>
                  </span>
                  <!-- 下拉的部分 -->
                  <!-- <div class="selectBlock pricingMode" v-if="showSelectPm">
                  <span v-for="(item,index) in pricingMode" :key="index" :title="item.title" @click.stop="select('showSelectPm','value6',item.title)" >{{item.title}}</span>
                  </div> -->
                  <div class="selectBlock pricingMode" v-if="showSelectUnit" style="bottom:33px">
                    <el-table :data="tableUnit" border size="mini"   style="width:251px;" @row-click='tbRowClickJLDW' 
                    @current-change="currentSelectJLDW"  
                    max-height="150" 
                    :header-row-style="headerStyle22" 
                    :fit="true" 
                    highlight-current-row 
                    ref="tableUnit" 
                    :row-class-name="tableRowClassName"
                    >
                      <el-table-column label="计量单位编码" prop="code" align="center" ></el-table-column>
                      <el-table-column label="计量单位名称" prop="unit" align="center"></el-table-column>
                      <!-- <el-table-column label="详细" align="center">
                        <template slot-scope='scope'>
                          <span @click.stop="findDetail(scope.row)" style="color:#409EFF;cursor:pointer">详细</span>
                        </template>
                      </el-table-column> -->
                    </el-table>
                    <div class="btnGroup">
                      <span class="add">增加</span>
                      <span class="listBoard">栏目</span>
                      <span class="all">全部</span>
                    </div>
                  </div>
                </div>
              </el-form-item>
                <el-form-item label="报表辅单位">
                  <el-select v-model="dialogForm.value6879" class="metering_drop-down_box" placeholder="请选择">
                    <el-option label="移动平均" value="移动平均"></el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="库存常用单位">
                  <el-select v-model="dialogForm.value68759" class="metering_drop-down_box" placeholder="请选择">
                    <el-option label="移动平均" value="移动平均"></el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="采购常用单位">
                  <el-select v-model="dialogForm.value6875789" class="metering_drop-down_box" placeholder="请选择">
                    <el-option label="移动平均" value="移动平均"></el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="销售常用单位">
                  <el-select v-model="dialogForm.value6434875789" class="metering_drop-down_box" placeholder="请选择">
                    <el-option label="移动平均" value="移动平均"></el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="零售常用单位">
                  <el-select v-model="dialogForm.value6875743489" class="metering_drop-down_box" placeholder="请选择">
                    <el-option label="移动平均" value="移动平均"></el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="生产常用单位">
                  <el-select v-model="dialogForm.value68757434819" class="metering_drop-down_box" placeholder="请选择">
                    <el-option label="移动平均" value="移动平均"></el-option>
                  </el-select>
                </el-form-item>
              </div>
              <div class="tabs_one_box_title">
                <span>存货属性</span>
                <div></div>
              </div>
              <div class="tabs_one_box_cont">
                <el-form-item label="  ">
                  <el-checkbox v-model="dialogForm.valu876198">外购</el-checkbox>
                  <el-checkbox v-model="dialogForm.valu8799812">销售</el-checkbox>
                  <el-checkbox v-model="dialogForm.valu8792">自制</el-checkbox>
                  <el-checkbox v-model="dialogForm.valu8755">生产耗用</el-checkbox>
                  <el-checkbox v-model="dialogForm.valu875566">劳务费用</el-checkbox>
                  <el-checkbox v-model="dialogForm.valu87785">成套件</el-checkbox>
                  <el-checkbox v-model="dialogForm.val45u87785">委外</el-checkbox>
                </el-form-item>
              </div>
              <div class="tabs_one_box_title">
                <span>其他信息</span>
                <div></div>
              </div>
              <div class="tabs_one_box_cont">
                <el-form-item label="存货描述">
                  <el-input v-model="dialogForm.value1167" style="width:700px" type="textarea"></el-input>
                </el-form-item>
              </div>
            </el-tab-pane>
            <el-tab-pane label="库存信息">
              <div class="tabs_one_box_title">
                <span>成本信息</span>
                <div></div>
              </div>
              <div class="tabs_one_box_cont">
                <el-form-item label="参考成本">
                  <el-input v-model.number="dialogForm.valu89568" class="metering_drop-down_box"></el-input>
                </el-form-item>
                <el-form-item label="最新成本">
                  <el-input v-model.number="dialogForm.valu898" class="metering_drop-down_box"></el-input>
                </el-form-item>
                <el-form-item label="平均成本">
                  <el-input v-model.number="dialogForm.valu899998" class="metering_drop-down_box"></el-input>
                </el-form-item>
              </div>
              <div class="tabs_one_box_title">
                <span>库存量信息</span>
                <div></div>
              </div>
              <div class="tabs_one_box_cont">
                <el-form-item label="最低库存">
                  <el-input v-model.number="dialogForm.hh" class="metering_drop-down_box"></el-input>
                </el-form-item>
                <el-form-item label="最高库存">
                  <el-input v-model.number="dialogForm.hhw" class="metering_drop-down_box"></el-input>
                </el-form-item>
                <el-form-item label="安全库存">
                  <el-input v-model.number="dialogForm.hwwhw" class="metering_drop-down_box"></el-input>
                </el-form-item>
                <el-form-item label="标准周转天数">
                  <el-input v-model.number="dialogForm.hwwhqw" class="metering_drop-down_box"></el-input>
                </el-form-item>
              </div>
              <div class="tabs_one_box_title">
                <span>其他信息</span>
                <div></div>
              </div>
              <div class="tabs_one_box_cont">
                <el-form-item label="默认供应商">
                  <!-- <el-select v-model="dialogForm.valueww" class="metering_drop-down_box" placeholder="请选择">
                    <el-option label="移动平均" value="移动平均"></el-option>
                  </el-select> -->
                  <div class="input_select" @click.stop="stopPop">
                  <!-- showSelectFocus 可作为公用方法，只要传递data待操作的data值就可以 -->
                    <input v-model="dialogForm.valueww" 
                    class="input_select_input" 
                    ref="mrgys"  
                    @focus="showSelectFocus('showSelectDefaultSupiler','defaultSuplier','indexMRGYS','mrgysCurrentRow')"
                    @input="filterTableList('valueww','defaultSuplier','oldDefaultSuplier','indexMRGYS',['correspondentUnitsCode','correspondentUnitsName','CustomerSettlement'])" 
                    @keydown.38="inputBlurChooseTableUp('defaultSuplier','indexMRGYS','mrgysCurrentRow')"  
                    @keydown.40="inputBlurChooseTableDown('defaultSuplier','indexMRGYS','mrgysCurrentRow')" 
                    @keyup.enter="inputEnter('showSelectDefaultSupiler','valueww','mrgysCurrentRow','correspondentUnitsName','mrgys')"
                    /> <!-- -->
                    <span class="input_select_select" @click.stop="showSelect('showSelectDefaultSupiler')">
                    <i class="el-icon-arrow-down"></i>
                    </span>
                    <div class="selectBlock pricingMode" v-if="showSelectDefaultSupiler" style="bottom:33px">
                      <el-table :data="defaultSuplier" border size="mini" style="width:350px"  
                      @row-click='tbRowClickMRGYS' 
                      @current-change="currentSelectMRGYS"  
                      max-height="150" 
                      :header-row-style="headerStyle22" 
                      :fit="true" 
                      highlight-current-row 
                      ref="defaultSuplier" 
                      :row-class-name="tableRowClassName"
                      >
                        <el-table-column label="往来单位编码" prop="correspondentUnitsCode" align="center" ></el-table-column>
                        <el-table-column label="往来单位名称" prop="correspondentUnitsName" align="center"></el-table-column>
                        <el-table-column label="结算客户" prop="CustomerSettlement" align="center"></el-table-column>
                        <el-table-column label="详细" align="center">
                          <template slot-scope='scope'>
                            <span @click.stop="findDetail(scope.row)" style="color:#409EFF;cursor:pointer">详细</span>
                          </template>
                        </el-table-column>
                      </el-table>
                      <div class="btnGroup">
                        <span class="add">增加</span>
                        <span class="listBoard">栏目</span>
                        <span class="all">全部</span>
                      </div>
                    </div>
                  </div>  
                </el-form-item>
                <el-form-item label="默认仓库">
                  <el-select v-model="dialogForm.valueqww" class="metering_drop-down_box" placeholder="请选择">
                    <el-option label="移动平均" value="移动平均"></el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="采购批量">
                  <el-input v-model="dialogForm.valuqeww" class="metering_drop-down_box"></el-input>
                </el-form-item>
                <el-form-item label="领料批量">
                  <el-input v-model="dialogForm.valuqeeqwww" class="metering_drop-down_box"></el-input>
                </el-form-item>
                <el-form-item label="领料批量处理方式">
                  <el-select v-model="dialogForm.valueqwwweq" class="metering_drop-down_box" placeholder="请选择">
                    <el-option label="移动平均" value="移动平均"></el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="入库上限比例%">
                  <el-input v-model="dialogForm.valuqeeqw75ww" class="metering_drop-down_box"></el-input>
                </el-form-item>
                <el-form-item label="出库上限比例%">
                  <el-input v-model="dialogForm.valuqeeqw7545ww" class="metering_drop-down_box"></el-input>
                </el-form-item>
                <el-form-item label=" " label-width="95px">
                  <el-checkbox v-model="dialogForm.valu898241">新品</el-checkbox>
                  <el-checkbox v-model="dialogForm.valu842349812">停用</el-checkbox>
                </el-form-item>
              </div>
            </el-tab-pane>
            <el-tab-pane label="图片">
              <!-- <picture-upload ref="pictureUpload"></picture-upload> -->
            </el-tab-pane>
          </el-tabs>
        </el-form>
      </div>
      <div class="add-footer">
        <el-button type="text" size="mini" style="color:#333">上张</el-button>
        <el-button type="text" size="mini" style="color:#333">下张</el-button>
        <el-button type="text" size="mini" style="color:#333">保存</el-button>
        <el-button type="text" size="mini" style="color:#333">保存新增</el-button>
        <el-button type="text" size="mini" style="color:#333">复制</el-button>
        <!-- <el-button type="text" size="mini" style="color:#333" @click.stop="addVisible = false">取消</el-button>
        <el-button type="text" size="mini" style="color:#333"  @click.stop="confirmTbColumnVisible">确定</el-button> -->
      </div>
      <!-- 计量单位弹窗 -->
      <!-- <metering-dialog ref="meteringDialog" @md-blue="onMdBlur"></metering-dialog> -->
    </el-dialog>
  </div>
</template>

<script>
// import meteringDialog from './metering/meteringDialog'
// import pictureUpload from './picture/pictureUpload'
export default {
//   components: {
//     meteringDialog,
//     pictureUpload
//   },
  data() {
    return {
      //表头样式没有生效
      headerStyle22:{
        'background':'rgb(173, 209, 243)'
      },
      //数据显示--所属分类
      tableAttrCato:[
        {flbm:'32132',flmc:'名称0',},
        {flbm:'r454',flmc:'名称1',},
        {flbm:'t454',flmc:'名称2',},
        {flbm:'3234df',flmc:'名称3',},
        {flbm:'3243',flmc:'名称4',},
        {flbm:'dn6767',flmc:'名称5',},
        {flbm:'dfsd323',flmc:'名称6',},
        {flbm:'dss3243',flmc:'名称7',},
      ],
      //计量单位
      tableUnit:[
        {code:'001',unit:'pcs'},
        {code:'002',unit:'yt'},
        {code:'003',unit:'cdd'},
        {code:'004',unit:'cdw'},
        {code:'005',unit:'cdc'},
        {code:'006',unit:'sds'},
      ],
      oldTableUnit:[
        {code:'001',unit:'pcs'},
        {code:'002',unit:'yt'},
        {code:'003',unit:'cdd'},
        {code:'004',unit:'cdw'},
        {code:'005',unit:'cdc'},
        {code:'006',unit:'sds'},
      ],
      //存储上面的属性类别的数据
      oldTableAttrCato:[
        {flbm:'32132',flmc:'名称0',},
        {flbm:'r454',flmc:'名称1',},
        {flbm:'t454',flmc:'名称2',},
        {flbm:'3234df',flmc:'名称3',},
        {flbm:'3243',flmc:'名称4',},
        {flbm:'dn6767',flmc:'名称5',},
        {flbm:'dfsd323',flmc:'名称6',},
        {flbm:'dss3243',flmc:'名称7',},
      ],
      //默认供应商
      defaultSuplier:[
        {correspondentUnitsCode:'001001',correspondentUnitsName:'测试供应商',CustomerSettlement:''},
        {correspondentUnitsCode:'001002',correspondentUnitsName:'美元供应商',CustomerSettlement:''},
        {correspondentUnitsCode:'001003',correspondentUnitsName:'材料供应商',CustomerSettlement:''},
      ],
      //默认供应商存储数据
      oldDefaultSuplier:[
        {correspondentUnitsCode:'001001',correspondentUnitsName:'测试供应商',CustomerSettlement:''},
        {correspondentUnitsCode:'001002',correspondentUnitsName:'美元供应商',CustomerSettlement:''},
        {correspondentUnitsCode:'001003',correspondentUnitsName:'材料供应商',CustomerSettlement:''},
      ],
      mdInputDisabled: false,
      setPostionVal: '',
      // 计价方式
      pricingMode:
      [
        {value:'全月平均',title:'全月平均'},
        {value:'移动平均',title:'移动平均'},
        {value:'先进先出',title:'先进先出'},
       {value:'个别计价',title:'个别计价'}
      ],
      showSelectPm:false,//所属分类
      showSelectUnit:false,//计量单位
      showSelectDefaultSupiler:false,//默认供应商的表
      data: [{
        label: '存货分类',
        children: [{
          label: '1000原材料',
          children: [{
            label: '1010电阻'
          }]
        }]
      }],
      defaultProps: {
        children: 'children',
        label: 'label'
      },
      tableData: [
        {
          cinvcode: 'HD0001',
          cinvName: '散热片螺丝',
          specifications: '镀锌 PM 3*7',
          pricingManner: '移动平均',
          category: '电阻',
          pricingUnit: 'PCS',
          referCost: '',
          brand: '潮牌',
          newCost: '0.39',
          newDate: '2019-07-23',
          mnemonicCode: '',
          selfControl: '',
          measuring: ''
        },
        {
          cinvcode: 'HD0002',
          cinvName: '散热片螺丝',
          specifications: '镀锌 PM 3*7',
          pricingManner: '移动平均',
          category: '电阻',
          pricingUnit: 'PCS',
          referCost: '',
          brand: '潮牌',
          newCost: '0.39',
          newDate: '2019-07-23',
          mnemonicCode: '',
          selfControl: '',
          measuring: ''
        },
        {
          cinvcode: 'HD0003',
          cinvName: '散热片螺丝',
          specifications: '镀锌 PM 3*7',
          pricingManner: '移动平均',
          category: '电阻',
          pricingUnit: 'PCS',
          referCost: '',
          brand: '潮牌',
          newCost: '0.39',
          newDate: '2019-07-23',
          mnemonicCode: '',
          selfControl: '',
          measuring: ''
        },
      ],
      headerStyle: {
        color: 'black'
      },
      tableColumn: [
        { label: '存货编码', prop: 'cinvcode', isShow: true, width: 100 },
        { label: '存货名称', prop: 'cinvName', isShow: true, width: 150 },
        { label: '规格型号', prop: 'specifications', isShow: true, width: 100 },
        { label: '计价方式', prop: 'pricingManner', isShow: true, width: 100 },
        { label: '所属类别', prop: 'category', isShow: true, width: 100 },
        { label: '品牌', prop: 'brand', isShow: true, width: 100 },
        { label: '计价单位', prop: 'pricingUnit', isShow: true, width: 100 },
        { label: '参考成本', prop: 'referCost', isShow: true, width: 100 },
        { label: '最新成本', prop: 'newCost', isShow: true, width: 100 },
        { label: '建档日期', prop: 'newDate', isShow: true, width: 100 },
        { label: '助记码', prop: 'mnemonicCode', isShow: false, width: 100 },
        { label: '自制', prop: 'selfControl', isShow: false, width: 100 },
        { label: '计量方式', prop: 'measuring', isShow: false, width: 100 }
      ],
      tableColumnBase: [
        { label: '存货编码', prop: 'cinvcode', isShow: true, width: 100 },
        { label: '存货名称', prop: 'cinvName', isShow: true, width: 150 },
        { label: '规格型号', prop: 'specifications', isShow: true, width: 100 },
        { label: '计价方式', prop: 'pricingManner', isShow: true, width: 100 },
        { label: '所属类别', prop: 'category', isShow: true, width: 100 },
        { label: '品牌', prop: 'brand', isShow: true, width: 100 },
        { label: '计价单位', prop: 'pricingUnit', isShow: true, width: 100 },
        { label: '参考成本', prop: 'referCost', isShow: true, width: 100 },
        { label: '最新成本', prop: 'newCost', isShow: true, width: 100 },
        { label: '建档日期', prop: 'newDate', isShow: true, width: 100 },
        { label: '助记码', prop: 'mnemonicCode', isShow: false, width: 100 },
        { label: '自制', prop: 'selfControl', isShow: false, width: 100 },
        { label: '计量方式', prop: 'measuring', isShow: false, width: 100 }
      ],
      pageRequest: {
        pageNum: 1,
        pageSize: 10,
      },
      dialogForm: {},
      tbColumnVisible: false,
      addVisible: false,
      currentRow: {},
      currentIndex: null,
      keydownCurrent:{},//当前行
      indexSSFL:0,//初始行
      indexJLDW:0,//初始行
      indexMRGYS:0,//初始行
      ssflCurrentRow:{},//所属分类
      jldwCurrentRow:{},//计量单位
      mrgysCurrentRow:{},//默认供应商
    };
  },
  methods: {
    //父级元素上的事件
    inventoryClick(){
      this.showSelectPm = false
      this.showSelectUnit = false
    },
    //可以复用（公用事件）
    stopPop(){
      //卸载父元素上的，阻止 冒泡
    },

    //右侧箭头的点击事件
    showSelect(value){
      //value 在此处是控制显隐的值
      //计价方式
      console.log(1)
      this[value] = !this[value]
    },
    //所属分类，
    showSelectFocus(value,tableNode,indexData,currentRow){
      //value 在此处是控制显隐的值
      // ref 与 data 里的数组同名，用同一个tableNode 
      console.log(value)
      console.log(this[value])
      this[value]= true
      this.$nextTick(()=>{
        console.log(this.indexSSFL)
        this.$refs[tableNode].setCurrentRow(this[tableNode][this[indexData]])
        this[currentRow] = this[tableNode][this.indexSSFL]
        this.$refs[tableNode].bodyWrapper.scrollTop = (this[indexData] - 2) * 23
      })
    },
    //所属分类
    tableRowClassName(row,rowIndex){
      row.row.index = row.rowIndex
      // console.log(row.row.index)
    },
    //setCurrentRow  tableAttrCato
    //上(所属分类)
    //Cannot read property '3' of undefined  报错
    inputBlurChooseTableUp(tableNode,indexData,currentRow){
      console.log("上")
      if(this[indexData] == 0){
        
      }else if(this[indexData] >= 1){
        this[indexData] = this[indexData] - 1
        console.log(this[tableNode])
        this.$refs[tableNode].setCurrentRow(this[tableNode][this[indexData]])
        this.currentRow = this[tableNode][this[indexData]]
      }
      if(this[indexData] <=3){
        this.$refs[tableNode].bodyWrapper.scrollTop = 0
      }
      if(this[indexData] >=3){
        this.$refs[tableNode].bodyWrapper.scrollTop = this.$refs[tableNode].bodyWrapper.scrollTop - 23
      }
      console.log(this.$refs[tableNode].bodyWrapper.scrollTop)
    },
    inputBlurChooseTableDown(tableNode,indexData,currentRow){
      //sslb
      console.log("JKDHWD")
      if(this[indexData] == this[tableNode].length -1){
        
      }else{
        this[indexData] = this[indexData] +  1
        this.$refs[tableNode].setCurrentRow(this[tableNode][this[indexData]])
        this[currentRow] = this[tableNode][this[indexData]]
      }
      console.log(this[indexData] >= 3 && this[indexData]<=this[tableNode].length -1)
      if(this[indexData] >= 3 && this[indexData]<this[tableNode].length -1){
        this.$refs[tableNode].bodyWrapper.scrollTop = this.$refs[tableNode].bodyWrapper.scrollTop + 23
      }
      console.log(this.$refs[tableNode].bodyWrapper.scrollTop)
    },
    //暂时没有使用blur事件
    inputSelectBlur(){
      //input失焦的事件
      console.log(this.ssflCurrentRow)
      // this.dialogForm.value6 = this.ssflCurrentRow.flbm
      // // this.$refs.tableAttrCato.$el.style.display = 'none'
      // this.showSelectPm = false
    },

    //所属分类
    inputEnter(value,setValue,currentRow,showWord,inputNode){
      //value 是控制显隐的值
      //setValue 与输入框绑定的值
      //enter事件
      console.log(this.$refs.inputNode)
      this.dialogForm[setValue] = this[currentRow][showWord]
      //回车的时候失去焦点
      console.log(this.$refs[inputNode])
      this.$refs[inputNode].blur()
      this[value] = false
    },
    //所属分类
    filterTableList(setValue,tableNode,oldTableArr,indexData,searchKeyWordArr){
      //setValue 绑定 input的值 
      //searchKeyWordArr 用来筛选的关键词的数组
      console.log(searchKeyWordArr)
      let arr = []
      if(this.dialogForm[setValue].length > 0){
        for(let i = 0 ;i<this[oldTableArr].length;i++){
          for(let j=0;j<searchKeyWordArr.length;j++){
            if(this[oldTableArr][i][searchKeyWordArr[j]].indexOf(this.dialogForm[setValue]) !== -1){
              arr.push(JSON.parse(JSON.stringify(this[oldTableArr][i])))
            }
          }
        }
        this[tableNode] = JSON.parse(JSON.stringify(arr))
        this[indexData] = 0
        this.$refs[tableNode].setCurrentRow(this[tableNode][this[indexData]])
      }
      if(this.dialogForm[setValue].length == 0) {
        this[tableNode] = JSON.parse(JSON.stringify(this[oldTableArr]))
        this[indexData] = 0
        this.$refs[tableNode].setCurrentRow(this[tableNode][this[indexData]])
      }
    },
    // <!--inputBlurChooseTableUp  -->
    // <!-- inputBlurChooseTableDown -->
    // <!-- filterCatoList -->

  // 大表格行点击 (所属分类) --加在表格上的事件，不进行封装
    tbRowClickSSFL(row,event,column){
      console.log('行点击');
      // this.tableData3.splice(this.nowIndex, 1, this.keydownCurrent);
      // this.keydownCurrent = {};
      this.indexSSFL = row.index
      this.ssflCurrentRow = this.tableAttrCato[row.index]
      this.dialogForm.value6 = this.ssflCurrentRow.flbm
      // 
      this.showSelectPm = false
    },
    //大表格行点击（计量单位）
    tbRowClickJLDW(row,event,column){
      console.log('行点击');
      // this.tableData3.splice(this.nowIndex, 1, this.keydownCurrent);
      // this.keydownCurrent = {};
      this.indexJLDW = row.index
      this.jldwCurrentRow = this.tableUnit[row.index]
      this.dialogForm.value7 = this.jldwCurrentRow.unit
      // 
      this.showSelectUnit = false
    },
    tbRowClickMRGYS(row,event,column){
      console.log('行点击');
      // this.tableData3.splice(this.nowIndex, 1, this.keydownCurrent);
      // this.keydownCurrent = {};
      this.indexMRGYS = row.index
      this.mrgysCurrentRow = this.defaultSuplier[row.index]
      this.dialogForm.valueww = this.mrgysCurrentRow.correspondentUnitsName
      // 
      this.showSelectDefaultSupiler = false
    },

  // 大表格当前行
  currentSelectSSFL(val,oldVal){
    console.log(val)
    // this.keydownCurrent = val; 
    //设置为当前行
    this.$refs.tableAttrCato.setCurrentRow(val)
    this.ssflCurrentRow = val;
  },
  currentSelectJLDW(val,oldVal){
    this.$refs.tableUnit.setCurrentRow(val)
    this.jldwCurrentRow = val;
  },
   currentSelectMRGYS(val,oldVal){
    this.$refs.defaultSuplier.setCurrentRow(val)
    this.mrgysCurrentRow = val;
  },
  //点击详情调用的接口
  findDetail(row){
    console.log(row)
    console.log('2332')
  },
    select(word,data,value){
      console.log(value)
      console.log(word)
      console.log(data)
      this.dialogForm[data] = value
      this[word] = false
    },
    // 计量单位 input 获取焦点（禁用 input）
    onMdFocus() {
      this.mdInputDisabled = true
      this.$refs.meteringDialog.open()
    },

    // 计量单位弹窗关闭时
    onMdBlur(tableRowData) {
      this.mdInputDisabled = false
      this.dialogForm.mdInputVal = tableRowData.name
    },

    handleNodeClick(data) {
      console.log(data);
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
    },
    findPosition() {
      this.$nextTick(() => {
        this.$refs.tree.setCheckedKeys([this.setPostionVal]);
      })
    },
    handleCurrentChange(val) {
      this.currentRow = val;
    },
    confirmTbColumnVisible() {
      this.tableColumn = JSON.parse(JSON.stringify(this.tableColumnBase))
      this.tbColumnVisible = false;
    },
    clickRow(ind) {
      console.log(ind)
      this.currentIndex = ind;
    },
    deleteTb() {
      console.log(this.currentRow)
      if (JSON.stringify(this.currentRow) === "{}" || !this.currentRow) {
        this.$message('请选择要删除的数据')
        return
      }
      let actions = () => {
        this.tableData.splice(this.currentIndex, 1);
        this.currentIndex = null;
        this.currentRow = {};
      }
      this.confirmMsg('删除', actions)
    }
  }
};
</script>

<style lang="scss" scoped>
.inventory {
  width: 100%;
  height: 100%;
  display: flex;
  padding: 5px;
  box-sizing: border-box;
  .inventory_left {
    width: 200px;
    height: 100%;
    border: 1px solid #cccccc;
    box-sizing: border-box;
    margin-right: 5px;
    .inventory_left_top {
      width: 100%;
      display: flex;
      padding: 5px;
      box-sizing: border-box;
      justify-content: space-between;
    }
    .inventory_left_tree_cont {
      width: 100%;
    }
  }
  .inventory_right {
    flex: 1;
    height: 100%;
    max-width: calc(100% - 200px);
    box-sizing: border-box;
    border: 1px solid #cccccc;
    .inventory_right_tools {
      width: 100%;
      height: 30px;
      border-bottom: 1px solid #cccccc;
      align-items: center;
      display: flex;
      padding: 0 20px;
      box-sizing: border-box;
    }
    .inventory_right_tools_btn {
      color: #606266;
      font-size: 14px;
      margin-left: 20px;
    }
    .pagination_line {
      height: 35px;
      margin-top: 5px;
      text-align: right;
    }
  }
}
.set_tab_box {
  width: 100%;
  height: 500px;
  overflow: auto;
}
.table_cont_p {
  width: 100%;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.el-table /deep/ .cell {
  padding: 0 !important;
  overflow: inherit !important;
  box-sizing: border-box;
}
/deep/.el-table--small td,
/deep/.el-table--small th {
  padding: 3px !important;
  box-sizing: border-box;
}
/deep/.el-table__body tr.current-row > td {
  background-color: #fdf5e6 !important;
}
.add-footer {
  width: 100%;
  border-top: 1px solid #eeeeee;
  text-align: right;
}
/deep/.el-dialog__body {
  padding-top: 10px !important;
  padding-bottom: 10px !important;
}
.el-form-item {
  margin-bottom: 5px;
}
.tabs_one_box_title {
  width: 100%;
  margin-bottom: 15px;
  display: flex;
  div {
    flex: 1;
    height: 8px;
    margin-left: 1%;
    border-bottom: 1px solid #cccccc;
  }
}
.tabs_one_box_cont {
  width: 100%;
  margin-bottom: 5px;
}
.metering_drop-down_box {
  width: 100px;
}
///原生input，select的样式设置
.input_select{
  border-radius:4px;
  border:1px solid #DCDFE6;
  position:relative;
}
.selectBlock{
  width:252px;
  position:absolute;
  // top:33px;
  left:0;
  display:flex;
  flex-direction: column;
  background:white;
  border:1px solid #DCDFE6;
  z-index:9999999;
  span{
    padding:0 10px;
    box-sizing: border-box;
    cursor: pointer;
  }
  span:hover{
    background:#F5F7FA;
  }
}
.btnGroup{
  display:flex;
  margin:5px;
  font-size:12px;
  span{
    height:20px;
    line-height:20px;
    border-radius: 5px;
    margin-left:8px;
    border:none;
    background:none;
    border:1px solid #DCDFE6;
    cursor: pointer;
  }
}
.input_select_input{
  width:210px;
  padding:0 15px;
  box-sizing: border-box;
  height:28px;
  border-radius:4px;
  border:none;
}
.input_select_select{
  display:inline-block;
  width:40px !important;
  height:28px;
  outline: none;
  border:none;
  border-left:1px solid #DCDFE6;
  cursor: pointer;
  text-align:center;
}
.input_select_select:hover{
  color:#409EFF;
}
.linkBg{
  background:#F5F7FA;
}
.headerStyle{
  background:rgb(173, 209, 243);
}
</style>