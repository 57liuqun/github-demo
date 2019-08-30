<template>
    <div class="element">
        <!-- element 组件的使用 -->
        <div class="displayBlock">
            <h2 class="blockTitle">图标</h2>
            <p><i class="el-icon-platform-eleme blockContent"></i></p>
        </div>
        <div class="displayBlock">
            <h2 class="blockTitle">按钮</h2>
            <el-button type="primary">primary</el-button>
            <el-button type="success">success</el-button>
            <el-button type="info">info</el-button>
            <el-button type="warning">warning</el-button>
            <el-button type="danger">danger</el-button>
            <el-button type="primary" round>round</el-button>
            <el-button type="danger" circle>circle</el-button>
            <el-button type="danger" circle>字多的圆形</el-button>
            <br />
            <el-button type="warning" plain>plain</el-button>
            <el-button type="primary" disabled>disabled</el-button>
            <el-button type="text">text</el-button>
            <el-button type="primary" icon="el-icon-edit">btnIcon</el-button>
            <el-button-group>
                <el-button type="danger">上一页(按钮组)</el-button>
                <el-button type="danger">下一页(按钮组)</el-button>
            </el-button-group>
            <el-button-group>
                <el-button type="primary"  icon="el-icon-edit" ></el-button>
                <el-button type="primary"  icon="el-icon-share" ></el-button>
                <el-button type="primary"  icon="el-icon-delete" ></el-button>
            </el-button-group>
            <el-button type="primary" :loading="true">loading</el-button>
            <el-button type="danger" >默认</el-button>
            <el-button type="danger" size="medium">medium</el-button>
            <el-button type="danger" size="small">small</el-button>
            <el-button type="warning" size="mini">mini</el-button>
            <el-button type="default" plain :autofocus="true">默认聚焦</el-button>
            <span>注:autofocus未生效</span>
        </div>
         <div class="displayBlock">
            <h2 class="blockTitle">原生属性</h2>
            <form>
                <input />
                <el-button native-type="reset">reset</el-button>
                <el-button native-type="reset">submit</el-button>
            </form>
        </div>
        <div class="displayBlock">
            <h2 class="blockTitle">文字链接</h2>
            <el-link type="primary">主要链接(primary)</el-link>
            <el-link type="primary" disabled>禁用链接</el-link>
            <el-link type="danger" :underline="false">无下划线</el-link>
            <el-link type="warning" icon="el-icon-edit">带图标的</el-link>
        </div>
        <div class="displayBlock">
            <h2 class="blockTitle">表单</h2>
            <div class="displayBlockItem">
                <h3 class="displayBlockItemTitle">单选框</h3>
                <span>这个单选框'[]' '[()]' 是什么操作 </span>
                <div class="displayBlockItemContent">
                    <!-- 绑定的model 与label的值相同的时候就会被选中 -->
                    <el-radio-group v-model="radio1">
                        <!-- 按钮样式的 -->
                        <el-radio-button label="上海">上海</el-radio-button>
                        <el-radio-button label="北京">北京</el-radio-button>
                        <el-radio-button label="广州">广州</el-radio-button>
                        <el-radio-button label="深圳">深圳</el-radio-button>
                    </el-radio-group>
                    <el-radio-group v-model="radio1">
                        <!-- 按钮样式的 -->
                        <el-radio label="上海">上海</el-radio>
                        <el-radio label="北京">北京</el-radio>
                        <el-radio label="广州">广州</el-radio>
                        <el-radio label="深圳">深圳</el-radio>
                    </el-radio-group>
                    <el-radio-group v-model="radio2" >
                        <!-- 按钮样式的 -->
                        <el-radio-button label="上海">上海</el-radio-button>
                        <el-radio-button label="北京">北京</el-radio-button>
                        <el-radio-button label="广州">广州</el-radio-button>
                        <el-radio-button label="深圳" >深圳</el-radio-button>
                    </el-radio-group>
                </div>
                <h3 class="displayBlockItemTitle">复选框</h3>
                <div class="displayBlockItemContent">
                    <el-checkbox-group v-model="checkList">
                        <el-checkbox v-for="(item,index) in cityList"  :key="index" :label="item.name" @change="pushItem(item.name)"
                        :disabled="item.name == '上海'"
                        >{{item.name}}</el-checkbox>
                    </el-checkbox-group>
                    <div style="margin:15px 0;"></div>
                    <!-- 带有全选按钮的 indeterminate 用以表示checkbox的不确定状态-->
                    <el-checkbox :indeterminate="isIndeterminate" v-model="checkAll" @change="checkCityAll">全选</el-checkbox>
                    <div style="margin:15px 0;"></div>
                    <el-checkbox-group v-model="checkList" @change="checkCity">
                        <el-checkbox v-for="city in cityList" :key="city.name" :label="city.name">{{city.name}}</el-checkbox>
                    </el-checkbox-group>
                    <!--  :min="1" :max="2"> 对选中的个数进行数量限制 ,只剩一个的时候,默认不可取消,超过3个,则不可再继续点击-->
                    <div style="margin:30px 0;"></div>
                    <el-checkbox-group v-model="checkList" :min="1" :max="3">
                        <el-checkbox v-for=" (item,index) in cityList" :key="index" :label="item.name">{{item.name}}</el-checkbox>
                    </el-checkbox-group>
                </div>
                <!-- 带边框的多选框 -->
            </div>
            <el-checkbox-group v-model="checkList" :min="1" :max="3" @change="ty">
                <el-checkbox v-for=" (item,index) in cityList" :key="index" :label="item.name" border>{{item.name}}</el-checkbox>
            </el-checkbox-group>
            <!-- label	选中状态的值（只有在checkbox-group或者绑定对象类型为array时有效） -->
        </div>
        <div class="displayBlock">
                <h3>input</h3>
                <div class="displayBlock">
                    <el-input v-model="input" style="width:100px" clearable ref="inputDemo" @focus="ty"/>
                    {{input}}
                    <el-input v-model="password" type="password" clearable show-password />
                    {{password}}
                    <!-- 带icon的输入框 predix-icon ,suffix-icon 放置-->
                    <el-input prefix-icon="el-icon-edit" v-model="icon" />
                    <el-input suffix-icon="el-icon-edit" v-model="icon" />
                    <!-- 通过slot放置icon 图标不竖直方向居中?????-->
                    <el-input v-model="icon" style="display:flex;align-items:center;"><i slot="suffix" class="el-icon-edit"></i></el-input>
                    <!-- 文本域 怎么设置不可resize呢-->
                    <el-input type="textarea" v-model="textarea" style="resize:none" :autosize="{minRows:2,maxRows:4}" ></el-input>
                    <!-- 复合型的输入框 -->
                    <el-input v-model="icon">
                        <!-- 加在前面 -->
                        <template slot="prepend">http://</template>
                    </el-input>
                    <el-input v-model="icon">
                        <!-- 加在后面 -->
                        <template slot="append"><i class="el-icon-edit"></i></template>
                    </el-input>
                    <!-- 一个前置下拉框的input -->
                    <el-input >
                        <el-select v-model="select"  slot="prepend" placeholder="请选择">
                            <el-option label="餐厅" value="1"></el-option>
                            <el-option label="2sa" value="2"></el-option>
                            <el-option label="eqwd" value="3"></el-option>
                            <el-option label="dsdd2d32" value="4"></el-option>
                        </el-select>
                    </el-input>
                    <!--一个带有搜索图标的input -->
                    <!-- native 用了elment组件,原生事件就需要加native -->
                    <el-input v-model="icon" @keyup.enter.native="serach">
                        <i slot="append" class="el-icon-search" @click="serach"></i>
                    </el-input>
                    <!-- 带输入建议的输入框 -->
                    <el-autocomplete 
                        v-model="auto1"
                        :fetch-suggestions="querySearch"
                        @select="handleSelect"
                    ></el-autocomplete>
                    <!-- 搜索后显示提示建议 -->
                    <el-autocomplete 
                        v-model="auto1"
                        :fetch-suggestions="querySearch"
                        @select="handleSelect"
                        :trigger-on-focus="false"
                    ></el-autocomplete>
                    <!-- 自定义输入建议的显示 -->
                    <el-autocomplete 
                        v-model="auto1"
                        :fetch-suggestions="querySearch"
                        @select="handleSelect"
                    >   
                    <!-- 作用域插槽 -->
                        <template slot-scope="{item}">
                            <div>{{item.value}}</div>
                            <div style="color:lightgray">{{item.number}}</div>
                        </template>
                    </el-autocomplete>
                    <!-- 限制长度并显示限制字数 -->
                    <el-input maxlength="10" placeholder="限制了字数" v-model='limit' show-word-limit>

                    </el-input>
                    <!-- readonly -->
                    <el-input readonly v-model="icon" placeholder="我是只读"></el-input>
                    <!-- validate-event -->
                    <el-input :validate-event="true" v-model="vinput"></el-input>
                    <!-- input 计数器 默认初始值为0 -->
                    <!-- 这个label有什么用  :disabled 禁用-->
                    <!-- <el-input-number v-model="numberCou" :min="1" :max="20" label="这是计数器" :disabled="true"></el-input-number> -->
                    <!-- 控制递增递减的步数 -->
                    <el-input-number v-model="numberCou" :min="1" :max="30" :step="6" ></el-input-number>
                    <!-- 严格步数,只能输入步数的倍数, 即是手动输入不是倍数的数字后会自动变为最接近的整数倍的数-->
                    <el-input-number v-model="numberCou" :step="2" step-strictly></el-input-number>
                    <!-- 控制输入的精度 ,只能控制,而不能阻止继续输入 precision -->
                    <span>控制精度</span>
                    <el-input-number v-model="strNum" :precision="2" maxlength="5"></el-input-number>
                    <!-- 不同尺寸的输入框 -->
                    <span>不同尺寸</span>
                    <el-input-number v-model="strNum" :precision="2" maxlength="5" size="medium"></el-input-number>
                    <el-input-number v-model="strNum" :precision="2" maxlength="5" size="small"></el-input-number>
                    <el-input-number v-model="strNum" :precision="2" maxlength="5" size="mini"></el-input-number>
                    <!-- 加减按钮的位置 -->
                    <!-- 当controls的值为false的时候,表示不使用控制器,即是一个一般的input框了 -->
                    <el-input-number v-model="strNum" :precision="2" maxlength="5" size="medium" :controls="false"></el-input-number>
                    <!-- 这个select怎么用 -->
                    <el-input-number v-model="strNum" :precision="2" maxlength="5" size="medium" controls-position="right" @select="selectFn"></el-input-number>
                </div>
                <h3>select</h3>
                <div class="displayBlock">
                    <!-- disabled 带禁用选项的 ,clearable 可清空选项,适用于单选-->
                    <el-select v-model="selected" placeholder="请选择店铺" @change="showSelect"  clearable >
                        <el-option v-for="(item,index) in selectListRes" :value="item.value" :label="item.label" :key="index" :disabled="item.disabled"></el-option>
                    </el-select>
                    <!-- 多选的select -->
                    <!-- 展示选择的全部 001-->
                    <el-select v-model="mulSel" placeholder="请选择店铺" @change="showSelect"  multiple >
                        <el-option v-for="(item,index) in selectListRes" :value="item.value" :label="item.label" :key="index" :disabled="item.disabled"></el-option>
                    </el-select>
                    <!-- 展示选择的数目 002-->
                    <el-select v-model="mulSel" placeholder="请选择店铺" @change="showSelect"  multiple collapse-tags >
                        <el-option v-for="(item,index) in selectListRes" :value="item.value" :label="item.label" :key="index" :disabled="item.disabled"></el-option>
                    </el-select>
                    <!-- 001 与 002  在展示上不同,其他一直一致-->
                    <input type="number" v-model="num" placeholder="请输入入" @input="setNum" step="10000"/>
                    <el-select v-model="mulSel" placeholder="请选择店铺" @change="showSelect"  multiple >
                        <el-option v-for="(item,index) in selectListRes" :value="item.value" :label="item.label" :key="index" :disabled="item.disabled">
                            <span>{{item.value}}</span>
                            <span>{{item.label}}</span>
                        </el-option>
                    </el-select>
                    <!-- select分组 -->
                    <!-- filterable 可输入进行,remote开启远程搜索 -->
                    <el-select v-model="mulSel" placeholder="请选择店铺" @change="showSelect"  multiple filterable remote
                    :remote-method="remoteMethod"
                    >
                        <el-option-group
                            v-for="(item,index) in selectGroup"
                            :key="index"
                            :label="item.label"
                        >
                            <el-option
                                v-for="(it,ind) in item.option"
                                :key="ind"
                                :value="it.value"
                                :label="it.label"
                            ></el-option>
                        </el-option-group>
                    </el-select>
                    <!-- 允许创建选项里没有的选项 -->
                    <!-- default-first-option 属性似乎没有起作用 -->
                    <el-select v-model="mulSel" placeholder="请选择店铺" @change="showSelect"  multiple allow-create filterable >
                        <el-option v-for="(item,index) in selectListRes" :value="item.value" :label="item.label" :key="index" :disabled="item.disabled">
                            <span>{{item.value}}</span>
                            <span>{{item.label}}</span>
                            <!-- 如果 Select 的绑定值为对象类型，请务必指定 value-key 作为它的唯一性标识。 -->
                        </el-option>
                    </el-select>
                    <!-- 绑定类型为object ,value-key 该怎么用,如何体现差异?? -->
                    <el-select v-model="mulSel" placeholder="请选择店铺" @change="showSelect"  multiple allow-create filterable >
                        <el-option v-for="(item,index) in selectListRes" :value="item" :label="item.label" :key="index" :disabled="item.disabled" :value-key="item">
                            <span>{{item.value}}</span>
                            <span>{{item.label}}</span>
                        </el-option>
                    </el-select>
                </div>
                <h3>级联</h3>
                <div class="displayBlock">
                    <!-- 级联的格式 label:'',value:'',children:[label:'',value:''] ,需要禁用则在字段声明disabled 即可-->
                    <el-cascader 
                        v-model="cascaValue"
                        :options="cascadList"
                        @change="handleChange"
                        clearable
                        placeholder="一般级联"
                    ></el-cascader>
                    <!-- :show-all-levels="false" 仅仅显示选项的最后一级 -->
                    <el-cascader 
                        v-model="cascaValue"
                        :options="cascadList"
                        @change="handleChange"
                        clearable
                        :show-all-levels="false" 
                        placeholder="仅显示最后一个选项的级联"
                    ></el-cascader>
                    <!-- props.multiplt="true"-->
                    <el-cascader 
                        v-model="cascaValue"
                        :options="cascadList"
                        @change="handleChange"
                        clearable
                        :show-all-levels="false" 
                        :props="{multiple:true}"
                        placeholder="可选"
                    ></el-cascader>
                    <!-- 选择任意一级选项 checkStrictly: true 取消父子的级联,multiple:true 负责多选-->
                    <el-cascader 
                        v-model="cascaValue"
                        :options="cascadList"
                        @change="handleChange"
                        clearable
                        :show-all-levels="false" 
                        :props="{multiple:true,checkStrictly: true}"
                        placeholder="选择任意一级的选项"
                    ></el-cascader>
                    <!-- 可动态加载下一级 -->
                    <el-cascader :props="props" placeholder="懒加载"></el-cascader>
                    <!-- 可搜索的级联 -->
                    <!-- checkStrictly: true -->
                    <el-cascader 
                        v-model="cascaValue"
                        :options="cascadList"
                        @change="handleChange"
                        clearable
                        :show-all-levels="false" 
                        :props="{multiple:true}"
                        placeholder="选择任意一级的选项"
                        filterable
                    ></el-cascader>
                    <!-- 自定义显示 -->
                    <el-cascader :options="cascadList" expandTrigger="hover">
                        <template slot-scope="{ node, data }">
                            <span>{{ data.label }}</span>
                            <span v-if="!node.isLeaf"> ({{ data.children.length }}) </span>
                        </template>
                    </el-cascader>
                </div>
                <h3>开关</h3>
                <div class="displayBlock">
                    <!-- 开关的默认颜色是 #409eff 蓝色 #dcdfe6 灰色 -->
                    <el-switch v-model="swValue"/>

                    <el-switch v-model="swValue" inactive-color="#0094e7" active-color="#ff9800" />
                    <!-- 加文字 -->
                    <el-switch v-model="swValue" active-text="like" inactive-text="dontlike"/>
                    <!-- 与tooltip结合使用 ,默认是在上方 placement-->
                    <el-tooltip :content="'选择是:'+swValue" placement="bottom" >
                        <!-- <el-switch v-model="swValue" active-text="like" inactive-text="dontlike"  /> -->
                        <!-- <el-switch v-model="swValue" active-text="like" inactive-text="dontlike"  active-value="sd" inactive-value="sa"/> -->
                        <el-switch v-model="swValue" active-text="like" inactive-text="dontlike"  active-value="sd" inactive-value="sa" active-icon-class="el-icon-edit" inactive-icon-class="el-icon-search"/>
                    </el-tooltip>
                </div>
            </div>
            <h3>滑块</h3>
            <div class="displayBlock">
                <!-- 默认 -->
                <el-slider v-model="sliderValue" style="width:200px;"></el-slider>
                <!-- 不显示提示 -->
                <el-slider v-model="sliderValue" style="width:200px;" :show-tooltip="false"></el-slider>
                <!-- 格式化提示 -->
                <el-slider v-model="sliderValue" style="width:200px;" :format-tooltip="format" ></el-slider>
                <!-- 滑块不可用 -->
                <el-slider v-model="sliderValue" style="width:200px;" disabled></el-slider>
                <!-- 离散值，即是可以间断 ,总值与step会冲突-->
                <el-slider v-model="sliderValue" style="width:200px" :step="step" show-stops></el-slider>
                <!-- 带有输入框的slider  -->
                <el-slider v-model="sliderValue" show-input style="width:500px"></el-slider>
                <!-- 范围选择,value绑定的是一个范围的值 -->
                <el-slider range v-model="sliderValueRange" style="width:200px"></el-slider>
                <!-- 竖向的滑块 ,必须设置height-->
                <el-slider v-model="sliderValue" vertical style="height:200px"></el-slider>
                <!-- 展开标记 -->
                <el-slider v-model="sliderValue" :marks="marks" style="width:200px"></el-slider>
            </div>
            <h3>时间选择器</h3>
            <div class="displayBlock">
                <!-- 各种类型的时间选择器 -->
                <!-- 1 提供固定的时间点让用户选择 -->
                <span>固定时间点</span>
                <!--  -->
                <el-time-select
                    style="width:200px"
                    v-model="timePicker"
                    :picker-options="{star:'08:30',step:'00:15',end:'18:30'}"
                    placeholder="选择时间"
                >
                </el-time-select>
                <!-- 一个时间范围内的任意 时间点 -->
                 <el-time-picker
                    style="width:200px"
                    v-model="value1"
                    :picker-options="{
                    selectableRange: '18:30:00 - 20:30:00'
                    }"
                    placeholder="任意时间点">
                </el-time-picker>
                <!-- arrow-control 加上箭头控制 ,就不能滑动了,贼傻吊的功能-->
                <el-time-picker
                    arrow-control
                    style="width:200px"
                    v-model="value1"
                    :picker-options="{
                    selectableRange: '18:30:00 - 20:30:00'
                    }"
                    ref="arrow-control-ep"
                    placeholder="任意时间点">
                </el-time-picker>
                <!-- 开始时间 与结束时间分开,但开始时间会影响结束时间的可选值  在options里面要设置 minTime: startTime 才能关联起来-->
                <el-time-select
                    placeholder="起始时间"
                    v-model="startTime"
                    :picker-options="{
                    start: '08:30',
                    step: '00:15',
                    end: '18:30'
                    }">
                </el-time-select>
                <el-time-select
                    placeholder="结束时间"
                    v-model="endTime"
                    :picker-options="{
                    start: '08:30',
                    step: '00:15',
                    end: '18:30',
                    minTime: startTime
                    }">
                </el-time-select>
                <!-- 时间范围 -->
                <!-- value-format	可选，仅TimePicker时可用，绑定值的格式。不指定则绑定值为 Date 对象	string	见日期格式 -->
                <el-time-picker
                    style="width:300px"
                    is-range
                    v-model="value3"
                    range-separator="至"
                    start-placeholder="开始时间"
                    end-placeholder="结束时间"
                    placeholder="选择时间范围"
                    @change="showTimeFormat"
                    value-format="h:m:s"
                >
                </el-time-picker>
                <!--日期选择器 -->
                <el-date-picker
                    style="width:200px"
                    v-model="valueDate"
                    type="date"
                    placeholder="选择日期"
                    :clearable="false"
                ></el-date-picker>
                <!-- 按周选择 -->
                <el-date-picker
                    style="width:200px"
                    v-model="valueDate1"
                    type="week"
                    format="yyyy 第 WW 周"
                    placeholder="选择周"
                ></el-date-picker>
                <!-- 选择月 -->
                <el-date-picker
                    style="width:200px;"
                    v-model="valueDate2"
                    type="month"
                    placeholder="请选择月"
                ></el-date-picker>
                <!-- 选择年 -->
                <el-date-picker
                    style="width:200px"
                    v-model="valueDate3"
                    type="year"
                    placeholder="选择年"
                ></el-date-picker>
                <!-- 选择一个日期或者多个日期 -->
                <el-date-picker 
                    style="width:200px"
                    v-model="valueDate4"
                    type="dates"
                    placeholder="选择一个或者多个日期"
                    @change="showMulDate"
                ></el-date-picker>
                <!-- 时间范围 -->
                <el-date-picker
                    v-model="dateRange"
                    type="daterange"
                    range-separator="至"
                    start-placeholder="开始日期"
                    end-placeholder="结束日期"
                    @change="showMulDate"
                ></el-date-picker>
                <!-- 带快捷选项的  unlink-panels 是什么-->
                <el-date-picker
                    v-model="dateRangeQuit"
                    type="daterange"
                    align="right"
                    range-separator="至"
                    start-placeholder="开始日期"
                    end-placeholder="结束日期"
                    :picker-options="pickerOptions"
                    unlink-panels
                    @change="showMulDate"
                ></el-date-picker>
                <!-- 选择月份范围 -->
                <el-date-picker
                    v-model="dateRange"
                    type="monthrange"
                    range-separator="至"
                    start-placeholder="开始日期"
                    end-placeholder="结束日期"
                    @change="showMulDate"
                ></el-date-picker>
                <!-- 选择日期时间 -->
                <el-date-picker
                    style="width:200px"
                    v-model="dateTime"
                    type="datetime"
                    placeholder="选择日期时间"
                    @change="showMulDate"
                    >
                </el-date-picker>
            </div>
            <h3>上传</h3>
            <div class="displayBlock">
                <el-upload 
                    action="https://jsonplaceholder.typicode.com/posts"
                    :on-preview="handlePreview"
                    :on-remove="handleRemove"
                    multiple
                    :limit="3"
                    :on-exceed="handleExceed"
                    :file-list="fileList"
                    :before-remove="beforeRemove"
                >
                    <el-button size="small" type="primary">点击上传</el-button>
                </el-upload>
                <!-- 上传头像 show-file-list-->
                <el-upload
                    action="https://jsonplaceholder.typicode.com/posts/"
                    :show-file-list="false"
                    :on-success="handleAvatarSuccess"
                    :before-upload="beforeAvatarUpLoad"
                    style="width:100px;height:100px;border:1px dashed lightgray;font-size:50px;display:flex;justify-content:center;align-items:center;color:gray;box-sizing:border-box"
                >
                    <img :src="imgUrl" v-if="imgUrl" class="avatar" style="width:95px;height:95px"/>
                    <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                </el-upload>
                <!-- 照片墙 -->
                <!-- list-type="picture-card" 必须写-->
                <el-upload
                    action="https://jsonplaceholder.typicode.com/posts/"
                    list-type="picture-card"
                    :on-preview="handlePictureCardPreview"
                    :on-remove="handleRemove">
                    <i class="el-icon-plus"></i>
                    </el-upload>
                    <el-dialog :visible.sync="dialogVisible">
                        <img width="100%" :src="dialogImageUrl" alt="">
                    </el-dialog>
                    <!-- 图片缩略列表 -->
                    <span>缩略图列表</span>
                    <el-upload
                        class="upload-demo"
                        action="https://jsonplaceholder.typeicode.com/posts"
                        :on-preview="handlePreview"
                        :on-remove="handleRemove"
                        :file-list="fileList"
                        list-type="picture"
                        :limit="4"
                    >
                        <el-button size="small" type="primary">点击上传</el-button>
                        <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
                    </el-upload>
                    <!-- 拖拽上传-->
                    <el-upload
                    @change="showlU"
                        class="upload-demo"
                        action="https://jsonplaceholder.typicode.com/posts/"
                        drag
                        :file-list="fileList"
                        multiple>
                        <i class="el-icon-upload"></i>
                        <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>
                        <div class="el-upload__tip" slot="tip">只能上传jpg/png文件，且不超过500kb</div>
                    </el-upload>
            </div>
            <h3>评分条</h3>
            <div class="displayBlock">
                <!-- :color="['#99a9bf','#f7ba2a','#ff9900']" 颜色区分 -->
                <!-- 写法2 :colors="{ 2: '#99A9BF', 4: { value: '#F7BA2A', excluded: true }, 5: '#FF9900' }" -->
                <el-rate v-model="rateValue" @change="showRATE"></el-rate>
                <span>颜色区分:</span>
                <el-rate v-model="rateValue" @change="showRATE" :colors="colors"></el-rate>
                <span>辅助文字:</span>
                <el-rate v-model="nullA" @change="showRATE" show-text :texts="tests"></el-rate>
                <!-- 使用图标来分阶 -->
                <span >图标表示</span>
                <!-- void-icon-class 未选择时的图标 -->
                <!-- 未显示是怎么回事 -->
                <!-- :icon-classes="iconclasses"
                    void-icon-class="icon-rate-face-off"
                    这两句没效果
                \ -->
                <!-- allow-half 允许半星 -->
                <el-rate
                    v-model="value22"
                    :colors="['#99A9BF', '#F7BA2A', '#FF9900']"
                    :allow-half="true"
                    >
                </el-rate>
                <!-- low-threshold(低) ,high-threshold(高) 区分分度等级的阀值 number -->
                <span>只读的评分心</span>
                <!-- show-score -->
                <!-- score-tempalte 分数显示模版 -->
                <el-rate v-model="value22" disabled show-score text-color="#ff9800" score-tempalte="{value22}" ></el-rate>
            </div>
            <h3>colorPicker</h3>
            <div class="displayBlock">
                <!-- show-alpha 多一个透明度的选择 -->
                <el-color-picker v-model="color1" show-alpha></el-color-picker>
                <input type="color"/>
                <!-- 预定义颜色  predefine-->
                <el-color-picker
                    v-model="color"
                    show-alpha
                    :predefine="predefineColors"
                ></el-color-picker>
                <!-- 不同尺寸的 -->
                <el-color-picker size="medium" v-model="colorAa"></el-color-picker>
                <el-color-picker size="small" v-model="colorAa"></el-color-picker>
                <el-color-picker size="mini" v-model="colorAa"></el-color-picker>
            </div>
            <h3>穿梭框</h3>
            <div class="displayBlock">
                <el-transfer v-model="value" :data="data">
                    <!-- 已经 自己进行封装(纯原生封装) -->
                </el-transfer>
            </div>
           <h3>form表单</h3>
           <div class="displayBlock">
               <span>行内表单</span>
               <el-form :inline="true" :model="formInline" class="demo-form-inline">
                   <el-form-item label="审批人">
                       <el-input v-model="formInline.user" placeholder="审批人" ></el-input>
                   </el-form-item>
                   <el-form-item label="活动区域">
                       <el-select v-model="formInline.region" placeholder="活动区域">
                           <el-option label="区域1" value="shanghai"></el-option>
                           <el-option label="区域2" value="beijing"></el-option>
                       </el-select>
                   </el-form-item>
                   <el-form-item>
                       <el-button type="primary" >提交</el-button>
                   </el-form-item>
               </el-form>
               <span>对齐方式</span>
               <!-- label-position -->
               <el-radio-group v-model="labelPosition" size="small" >
                   <el-radio-button label="left">左对齐</el-radio-button>
                   <el-radio-button label="right">右对齐</el-radio-button>
                   <el-radio-button label="top">顶部对齐</el-radio-button>
               </el-radio-group>
               <!-- label-position -->
               <el-form :label-position="labelPosition" label-width="80px" :model="formLabelAlign">
                   <el-form-item label="名称">
                       <el-input v-model="formLabelAlign.name"></el-input>
                   </el-form-item>
                   <el-form-item label="活动区域">
                       <el-input v-model="formLabelAlign.region"></el-input>
                   </el-form-item>
                   <el-form-item label="活动形式">
                       <el-input v-model="formLabelAlign.type"></el-input>
                   </el-form-item>
               </el-form>
               <span>表单验证</span>
               <div class="displayBlock">
                   <!-- ref="ruleForm" 的设置 -->
                   <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
                       <!-- rules : type: 'date', required: true, message: '请选择日期', trigger: 'change' -->
                        <el-form-item label="活动名称" prop="name">
                            <el-input v-model="ruleForm.name"></el-input>
                        </el-form-item>
                        <el-form-item label="活动区域" prop="region">
                            <el-select v-model="ruleForm.region" placeholder="请选择活动区域">
                                <el-option label="区域1" value="shanghai"></el-option>
                                <el-option label="区域2" value="beijing"></el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="活动时间" required>
                            <el-col :span="11">
                                <el-form-item prop="date1">
                                    <el-date-picker type="date" placeholder="请选择日期" v-model="ruleForm.date1" style="width:100%"></el-date-picker>
                                </el-form-item>
                            </el-col>
                            <el-col  class="line" :span="2" >-</el-col>
                            <el-col :span="11" >
                                <el-form-item prop="date2" >
                                    <el-time-picker placeholder="选择时间" v-model="ruleForm.date2" style="width:100%" > </el-time-picker>
                                </el-form-item>
                            </el-col>
                        </el-form-item>
                        <el-form-item label="活动性质" prop="type">
                            <el-checkbox-group v-model="ruleForm.type">
                                <el-checkbox label="美食/餐厅线上活动" name="type"></el-checkbox>
                                <el-checkbox label="地推活动" name="type"></el-checkbox>
                                <el-checkbox label="线下主题活动" name="type"></el-checkbox>
                                <el-checkbox label="单纯品牌曝光" name="type"></el-checkbox>
                            </el-checkbox-group>
                        </el-form-item>
                        <el-form-item label="特殊资源" prop="resource">
                            <el-radio-group v-model="ruleForm.resource">
                                <el-radio label="线上品牌赞助"></el-radio>
                                <el-radio label="线上场地免费"></el-radio>
                            </el-radio-group>
                        </el-form-item>
                        <el-form-item>
                            <el-button type="primary" @click="submitForm('ruleForm')" >立即创建</el-button>
                            <el-button @click="resetForm('ruleForm')">重置</el-button>
                        </el-form-item>
                   </el-form>
                   <!-- 自定义校验规则 -->
                   <span>自定义校验规则</span>
                   <el-form :model="ruleForm2" status-icon :rules="rules2" ref="ruleForm2" label-width="100px" class="demo-ruleForm">
                        <el-form-item label="密码" prop="pass">
                            <el-input type="password" v-model="ruleForm2.pass" autocomplete="off"></el-input>
                        </el-form-item>
                        <el-form-item label="确认密码" prop="checkPass">
                            <el-input type="password" v-model="ruleForm2.checkPass" autocomplete="off"></el-input>
                        </el-form-item>
                        <el-form-item label="年龄" prop="age">
                            <el-input v-model.number="ruleForm2.age"></el-input>
                        </el-form-item>
                        <el-form-item>
                            <el-button type="primary" @click="submitForm('ruleForm2')">提交</el-button>
                            <el-button @click="resetForm('ruleForm2')">重置</el-button>
                        </el-form-item>
                    </el-form>
                    <!-- 动态增减表单项 -->
                    <span>动态增减表单项</span>
                    
               </div>
           </div>
    </div>
</template>
<script>
let id = 0
export default {
   data () {
    //    下划线与 () 的区别
       const generateData = _ =>{
           const data = []
           for(let i = 1;i <= 15; i++){
               data.push({
                   key:i,
                   label:`备选项${i}`,
                   disabled:i%2 === 0
               })
           }
            return data
       }
       const checkAge = (rule,value,callback)=>{
           if(!value){
               return callback(new Error('年龄不能为空'))
           }
           if(!Number.isInteger(value)){
               calllback(new Error('请输入数字值'))
           }else{
               if(value < 18){
                   callback('必须年满18岁')
               }else{
                   callback()
               }
           }
       }
    //    自定义校验 callback 必须被调用。
       const valiPass = (rule,value,callback)=>{
           if(value === ''){
               callback(new Error('请输入密码'))
           }else{
               if(this.ruleForm2.checkPass !== ''){
                   this.$refs.ruleForm2.validateField('checkPass')
               }
               callback()
           }
       }
       const valiPass2 = (rule,value,callback)=>{
           if(value === ''){
               callback(new Error('请再次输入密码'))
           }else if(value !== this.ruleForm2.pass){
               callback(new Error('两次输入的密码不一致'))
           }else {
               callback()
           }
       }
       return {
           formInline:{
               user:'',
               region:''
           },
           formLabelAlign:{
               name:'',
               region:'',
               type:''
           },
           ruleForm:{
               name:'',
               region:'',
               date1:'',
               date2:'',
               deliver:false,
               type:[],
               resource:[],
               desc:''
           },
        //    验证规则
            rules:{
               name:[
                   {required:true,message:'请输入活动名称',trigger:'blur'},
                   {min:3,max:5,message:'长度3到5个字符',trigger:'blur'}
               ],
               region:[
                   {reuire:true,message:'请选择活动区域',trigger:'change'},
               ],
               date1: [
                    { type: 'date', required: true, message: '请选择日期', trigger: 'change' }
                ],
                date2: [
                    { type: 'date', required: true, message: '请选择时间', trigger: 'change' }
                ],
                type: [
                    { type: 'array', required: true, message: '请至少选择一个活动性质', trigger: 'change' }
                ],
                resource: [
                    { required: true, message: '请选择活动资源', trigger: 'change' }
                ],
                desc: [
                    { required: true, message: '请填写活动形式', trigger: 'blur' }
                ]
            },
            rules2:{
                // 自定义校验规则
                pass:[
                    {validator:valiPass,trigger:'blur'},
                ],
                checkPass:[
                    {validator:valiPass2,trigger:'blur'},
                ],
                age:[
                    {validator:checkAge,trigger:'blur'}
                ]
            },
            ruleForm2:{
                pass: '',
                checkPass: '',
                age: ''
            },
           labelPosition:'left',
           data:generateData(),
           value:[1,4],
           color:'#757348',
           colorAa:'409EFF',
           predefineColors:['#676897','#675533','#235757'],
           color1:'',
           value22:0.5,
           iconclasses:['icon-rate-face-1','icon-rate-face-2','icon-rate-face-3'],
           nullA:null,
        //    长度要等于max
           tests:['0','1','2','3','4'],
           colors:['#99a9bf','#f7ba2a','#ff9900'],
           rateValue:'',
           dialogImageUrl: '',
            dialogVisible: false,
           imgUrl:'',//图片地址
           fileList:[],//上传
           dateTime:'',
           dateRangeQuit:'',
           pickerOptions:{
            //    快捷选项的数据设置
            // shortcuts	设置快捷选项，需要传入 { text, onClick } 对象用法参考 demo 或下表
            shortcuts:[
                {
                    text:'最近一周',
                    onClick(picker){
                        const end = new Date()
                        const start = new Date()
                        start.setTime(start.getTime() - 3600 * 1000 *24 *7 )
                        picker.$emit('pick',[start,end])
                    }
                },
                {
                    text:'最近三天',
                    onClick(picker){
                        console.log(picker)
                        const end = new Date()
                        const start = new Date()
                        start.setTime(start.getTime() - 3600 * 1000 * 24 * 3)
                        picker.$emit('pick',[start,end])
                    }
                },
                {
                    text:'最近三个月',
                    onClick(picker){
                        const end = new Date()
                        const start = new Date()
                        start.setTime(start.getTime() - 3600 * 1000 * 24 * 90)
                        picker.$emit('pick',[start,end])
                    }
                }
            ]
           },
           dateRange:'',
           valueDate1:'',
           valueDate2:'',
           valueDate3:'',
           valueDate4:'',
           valueDate:'',
           value3: [new Date(2016, 9, 10, 8, 40), new Date(2016, 9, 10, 9, 40)],
           startTime:'',
           endTime:'',
           step:30,
           value1:new Date(2016, 9, 10, 18, 40),
           timePicker:'',
           marks:{
               0:'0c',
               50:'高温',
           },
           sliderValueRange:[4,50],
           sliderValue:20,
           swValue:true,
           props:{
               lazy:true,
               lazyLoad(node,resolve){
                   const {level} = node
                   console.log(node)
                   console.log(level)
                   setTimeout(()=>{
                       const nodes = Array.from({length:level+1})
                       .map(item =>({
                           value:++id,
                           label:`选项${id}`,
                           leaf:level>=2
                       }))
                       resolve(nodes)
                   },1000)
               }
           },
           cascaValue:'',
           cascadList:[
               {label:'xihuanni',value:'2312321',disabled:true,children:[{label:'xihuanic',value:'3423423',children:[{label:'xihuannic1',value:'324234'}] }]},
               {label:'xihuanni',value:'2312321',children:[{label:'xihuanic',value:'3423423',children:[{label:'xihuannic1',value:'324234',disabled:true}] }]},
               {label:'xihuanni',value:'2312321',children:[{label:'xihuanic',value:'3423423',children:[{label:'xihuannic1',value:'324234'}] }]},
               {label:'xihuanni',value:'2312321',children:[{label:'xihuanic',value:'3423423',children:[{label:'xihuannic1',value:'324234'}] }]},
           ],
           selectGroup:[
               {label:'group1',option:[
                   {value:'46',label:'dsds'},
                   {value:'534',label:'ada'},
                   {value:'21',label:'zxzc'},
                   {value:'234',label:'czcz'},
               ]},
               {label:'group2',option:[
                   {value:'12',label:'fcbc'},
                   {value:'456',label:'dfsd'},
                   {value:'57',label:'dsfdsf'},
                   {value:'757',label:'sdfsd'},
               ]},
           ],
           num:'',
           mulSel:[],//duoxuan
           selected:'',
           selectListRes:[
               {value:1,label:'老虎山',disabled:false},
               {value:2,label:'益和堂',disabled:false},
               {value:3,label:'奈雪',disabled:true},
               {value:4,label:'coco',disabled:false},
               {value:5,label:'蜜雪冰城',disabled:false},
           ],
           radio1:"上海",
           radio2:"广州",
           checkList:['北京','上海'],
           cityList:[
               {name:'北京',url:''},
               {name:'上海',url:''},
               {name:'广州',url:''},
               {name:'深圳',url:''},
            ],
            isIndeterminate:true,//
            checkAll:true,
            input:'',//输入框的绑定值
            password:'23123',//密码,
            icon:'213',
            textarea:'',
            select:'',
            auto1:'',
            limit:"",
            vinput:'',
            numberCou:1,
            strNum:1.00
       }
   },
   mounted(){
       console.log(new Date(new Date().getFullYear(),0))
       console.log(new Date('2019-08-28 09:09:09'))
       console.log(this.$refs)
       console.log(this.value3)
   },
   methods:{
       submitForm(name){
           this.$refs[name].validate((valid)=>{
               if(valid){
                   alert('submit')
               }else{
                   alert('error submit')
                   return false
               }
           })
       },
       resetForm(name){
           console.log(this.$refs[name])
           this.$refs[name].resetFields()
       },
       showRATE(e){
           //打印出来的值即是评分
           console.log(e)
       },
       showlU(){
           console.log(this.fileList)
       },
       handlePictureCardPreview(file){
        //   打开遮罩，链接赋值
           this.dialogImageUrl = file.url;
            this.dialogVisible = true;
       },
       handleAvatarSuccess(res,file){
           console.log("handleAvatarSuccess")
           console.log(res)
           console.log(file)
           //此特性在 Web Worker 中可用。
            // 注意：此特性在 Service Worker 中不可用
            //用于创建 URL 的 File 对象、Blob 对象或者 MediaSource 对象。​
           this.imgUrl = URL.createObjectURL(file.raw)
        //    在每次调用 createObjectURL() 方法时，都会创建一个新的 URL 对象，即使你已经用相同的对象作为参数创建过。
        //    当不再需要这些 URL 对象时，
        //    每个对象必须通过调用 URL.revokeObjectURL() 方法来释放。浏览器会在文档退出的时候自动释放它们，
        //    但是为了获得最佳性能和内存使用状况，你应该在安全的时机主动释放掉它们。
       },
       beforeAvatarUpLoad(file){
           console.log('beforeAvatarUpLoad')
           const isJpg = file.type === 'image/jpeg';
           const is2m = file.size /1024 /1024 < 2
           if(!isJpg){
               this.$message.error("只能上传jpeg类型的图片")
           }
           if(!is2m){
               this.$message.error("不支持超过2m的图片")
           }
           return isJpg && is2m
       },
       beforeRemove(val){
           console.log('beforeRemove')
           //返回一个boolean 确认是否删除
           return false
       },
       handlePreview(){
           console.log('handlePreview')
       },
       handleRemove(){
           console.log('handleRemove')
       },
       handleExceed(){
           this.$message(
               '最多只能上传三个')
       },
       showMulDate(e){
           console.log(e)
       },
       showTimeFormat(e){
           console.log(e)
       },
       format(val){
           return val/100.00
       },
       handleChange(){
           console.log("喜欢你")
       },
       remoteMethod(){
        //    change事件
           console.log(1)
       },
       setNum(){
           this.num = Number(this.num.slice(0,5))
           console.log(this.num)
       },
       showSelect(e){
           console.log(e)
       },
       selectFn(){
           console.lgo('1321')
       },
       checkReg(){
           let reg = /\d{3}/
           if(!reg.test(this.vinput)){
               return false
           }else{
               return true
           }
       },
       querySearch(strting,cb){
           //使用string 进行请求搜索,将请求到的数据丢进callback
           let result = [{value:'323232',number:'13425678903'},{value:'3433',number:'13425678903'},{value:'32432434',number:'13425678903'}]
           cb(result)
           console.log(cb)
       },
        handleSelect(){},
       serach(){
           alert("搜索")
       },
       ty(){
           console.log('ty')
           console.log(this.$refs.inputDemo.$refs.input)
       },
       checkCityAll(e){
           console.log(e)
           let allList = []
           this.cityList.forEach(element => {
               allList.push(element.name)
           });
           this.checkList = this.checkAll ? allList : [];
           this.isIndeterminate = false;
       },
       checkCity(e){
            let checkedCount = e.length;
            console.log(checkedCount)
            this.checkAll = checkedCount === this.cityList.length;
            console.log(this.checkAll)
            this.isIndeterminate = checkedCount > 0 && checkedCount < this.cityList.length;
       }
   } 
}
</script>
<style scoped land="scss">
    .displayBlock{
        display:flex;
        max-width:900px;
        flex-wrap:wrap;
        align-items: center;
       
    }
    .displayBlock .el-input{
        width:100px;
    }
     .displayBlockItem{
        display:flex;
        align-items:center;
    }
    .blockTitle{
        margin-right:20px;
    }
</style>