<template>

  <keep-alive>

  <!--首页组件-->
  <div id="content" style="margin-top: 0px;" >

    <header id="wx-header" v-if="iswechat" >
        <div class="center" >
            <router-link :to="back" @click="$router.push(`/app`)" tag="div" class="iconfont icon-left">
                <span>返回</span>
            </router-link>
            <span>奖罚申请</span>
            <van-dropdown-menu id="header-drop-menu" class="header-drop-menu" @change="headDropMenu();" z-index="100" style="position: absolute; width: 45px; height: auto; right: -15px; top: -3px; opacity: 1; background:#1b1b1b; ">
              <van-icon name="weapp-nav" size="1.3rem" @click="headMenuToggle" style="position: absolute; width: 40px; height: auto; right: 15px; top: 16px; opacity: 1; background:#1b1b1b;z-index:10000; " />
              <van-dropdown-item v-model="dropMenuValue" ref="headMenuItem" :options="dropMenuOption" @change="headDropMenu();" />
            </van-dropdown-menu>
        </div>
    </header>

    <section v-if="iswechat">

      <div class="weui-cells" style="margin-top:0px;">

        <div class="weui-cells" style="margin-top:0px;border-bottom:0px solid #fefefe;">
          <van-notice-bar
              v-show=" title!='' && title != null && typeof title != 'undefined' "
              left-icon="volume-o"
              color="#1989fa"
              background="#ecf9ff"
              :text="title"
            />
        </div>

        <div class="" id="scanCell" style="padding: 8px 10px 4px 10px;">
          <van-row>
            <van-col span="8"></van-col>
            <van-col span="8" style="text-align: center;font-size:1.15rem;">奖罚申请表</van-col>
            <van-col span="8"></van-col>
          </van-row>
        </div>

      </div>

      <div class="wechat-list" style="background-color:#fefefe;margin-top:0px;border-bottom:0px solid #fefefe;">
        <div class="weui-cells" style="margin-top:0px;border-bottom:0px solid #fefefe;">
        </div>
        <div class="weui-cells" style="margin-top:0px;margin-left:10px;padding-top:5px;padding-bottom:15px;border-bottom:0px solid #fefefe;">

          <van-cell-group>

            <van-form >

              <van-cell-group style="margin-top:10px;position:relative;">

                <van-cell value="基础信息" style="margin-left:0px;margin-left:-3px;font-size: 0.95rem;" />

                <van-field v-show="item.serialid" clearable label="流水序号" v-model="item.serialid" placeholder="系统自动生成序号！" readonly />
                <!-- 借用时间（HR需要确认/修改） -->
                <van-field :readonly="true" :required="false" clearable label="借用时间" v-model="item.receive_time"  placeholder="请填写借用时间！" @blur="validField('receive_time')" :error-message="message.receive_time"  />
                <!-- 借用类别（HR需要确认/修改） -->
                <van-field :readonly="readonly" :required="false" clearable label="借用类别" v-model="item.type"  placeholder="请填写借用类别！" @blur="validField('type')" :error-message="message.type"  />
                <!-- 物品名称（HR需要确认/修改） -->
                <van-field :readonly="readonly" required clearable :label="item.type == '传屏设备' ? '传屏编号': '设备名称'" v-model="item.name"  :placeholder="item.type == '传屏设备' ? '请填写传屏编号!': '请填写设备名称!'" @blur="validField('name')" :error-message="message.name"  />
                <!-- 借用数量（HR需要确认/修改） -->
                <van-field :readonly="readonly" required clearable label="借用数量" v-model="item.amount"  placeholder="请填写借用数量及单位！" @blur="validField('amount')" :error-message="message.amount"  />

                <van-icon name="add-o" style="position:absolute;top:115px;right:0px;" @click="size <= 16 ? size++ : size;"/>

                <van-icon name="circle" style="position:absolute;top:155px;right:0px;" @click="size > 1 ? size-- : size;"  />

                <span class="van-goods-span-number" style="top:130px;">#1</span>
              </van-cell-group>

              <van-cell-group v-show="size>=2" style="margin-top:10px;position:relative;border-top:0px solid #fefefe;">

                <!-- 物品名称（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable :label="item.type == '传屏设备' ? '传屏编号': '设备名称'" v-model="item.name1"  :placeholder="item.type == '传屏设备' ? '请填写传屏编号!': '请填写设备名称!'" @blur="validField('name')" :error-message="message.name"  />
                <!-- 借用数量（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable label="借用数量" v-model="item.amount1"  placeholder="请填写借用数量！" @blur="validField('amount')" :error-message="message.amount"  />

                <span class="van-goods-span-number">#2</span>
              </van-cell-group>

              <van-cell-group v-show="size>=3" style="margin-top:10px;position:relative;border-top:0px solid #fefefe;">

                <!-- 物品名称（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable :label="item.type == '传屏设备' ? '传屏编号': '设备名称'" v-model="item.name2"  :placeholder="item.type == '传屏设备' ? '请填写传屏编号!': '请填写设备名称!'" @blur="validField('name')" :error-message="message.name"  />
                <!-- 借用数量（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable label="借用数量" v-model="item.amount2"  placeholder="请填写借用数量！" @blur="validField('amount')" :error-message="message.amount"  />

                <span class="van-goods-span-number">#3</span>
              </van-cell-group>

              <van-cell-group v-show="size>=4" style="margin-top:10px;position:relative;border-top:0px solid #fefefe;">

                <!-- 物品名称（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable :label="item.type == '传屏设备' ? '传屏编号': '设备名称'" v-model="item.name3"  :placeholder="item.type == '传屏设备' ? '请填写传屏编号!': '请填写设备名称!'" @blur="validField('name')" :error-message="message.name"  />
                <!-- 借用数量（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable label="借用数量" v-model="item.amount3"  placeholder="请填写借用数量！" @blur="validField('amount')" :error-message="message.amount"  />

                <span class="van-goods-span-number">#4</span>
              </van-cell-group>

              <van-cell-group v-show="size>=5" style="margin-top:10px;position:relative;border-top:0px solid #fefefe;">

                <!-- 物品名称（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable :label="item.type == '传屏设备' ? '传屏编号': '设备名称'" v-model="item.name4"  :placeholder="item.type == '传屏设备' ? '请填写传屏编号!': '请填写设备名称!'" @blur="validField('name')" :error-message="message.name"  />
                <!-- 借用数量（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable label="借用数量" v-model="item.amount4"  placeholder="请填写借用数量！" @blur="validField('amount')" :error-message="message.amount"  />

                <span class="van-goods-span-number">#5</span>
              </van-cell-group>

              <van-cell-group v-show="size>=6" style="margin-top:10px;position:relative;border-top:0px solid #fefefe;">

                <!-- 物品名称（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable :label="item.type == '传屏设备' ? '传屏编号': '设备名称'" v-model="item.name5"  :placeholder="item.type == '传屏设备' ? '请填写传屏编号!': '请填写设备名称!'" @blur="validField('name')" :error-message="message.name"  />
                <!-- 借用数量（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable label="借用数量" v-model="item.amount5"  placeholder="请填写借用数量！" @blur="validField('amount')" :error-message="message.amount"  />

                <span class="van-goods-span-number">#6</span>
              </van-cell-group>

              <van-cell-group v-show="size>=7" style="margin-top:10px;position:relative;border-top:0px solid #fefefe;">

                <!-- 物品名称（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable :label="item.type == '传屏设备' ? '传屏编号': '设备名称'" v-model="item.name6"  :placeholder="item.type == '传屏设备' ? '请填写传屏编号!': '请填写设备名称!'" @blur="validField('name')" :error-message="message.name"  />
                <!-- 借用数量（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable label="借用数量" v-model="item.amount6"  placeholder="请填写借用数量！" @blur="validField('amount')" :error-message="message.amount"  />

                <span class="van-goods-span-number">#7</span>
              </van-cell-group>

              <van-cell-group v-show="size>=8" style="margin-top:10px;position:relative;border-top:0px solid #fefefe;">

                <!-- 物品名称（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable :label="item.type == '传屏设备' ? '传屏编号': '设备名称'" v-model="item.name7"  :placeholder="item.type == '传屏设备' ? '请填写传屏编号!': '请填写设备名称!'" @blur="validField('name')" :error-message="message.name"  />
                <!-- 借用数量（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable label="借用数量" v-model="item.amount7"  placeholder="请填写借用数量！" @blur="validField('amount')" :error-message="message.amount"  />

                <span class="van-goods-span-number">#8</span>
              </van-cell-group>

              <van-cell-group v-show="size>=9" style="margin-top:10px;position:relative;border-top:0px solid #fefefe;">

                <!-- 物品名称（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable :label="item.type == '传屏设备' ? '传屏编号': '设备名称'" v-model="item.name8"  :placeholder="item.type == '传屏设备' ? '请填写传屏编号!': '请填写设备名称!'" @blur="validField('name')" :error-message="message.name"  />
                <!-- 借用数量（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable label="借用数量" v-model="item.amount8"  placeholder="请填写借用数量！" @blur="validField('amount')" :error-message="message.amount"  />

                <span class="van-goods-span-number">#9</span>
              </van-cell-group>

              <van-cell-group v-show="size>=10" style="margin-top:10px;position:relative;border-top:0px solid #fefefe;">

                <!-- 物品名称（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable :label="item.type == '传屏设备' ? '传屏编号': '设备名称'" v-model="item.name9"  :placeholder="item.type == '传屏设备' ? '请填写传屏编号!': '请填写设备名称!'" @blur="validField('name')" :error-message="message.name"  />
                <!-- 借用数量（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable label="借用数量" v-model="item.amount9"  placeholder="请填写借用数量！" @blur="validField('amount')" :error-message="message.amount"  />

                <span class="van-goods-span-number">#10</span>
              </van-cell-group>

              <van-cell-group v-show="size>=11" style="margin-top:10px;position:relative;border-top:0px solid #fefefe;">

                <!-- 物品名称（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable :label="item.type == '传屏设备' ? '传屏编号': '设备名称'" v-model="item.name10"  :placeholder="item.type == '传屏设备' ? '请填写传屏编号!': '请填写设备名称!'" @blur="validField('name')" :error-message="message.name"  />
                <!-- 借用数量（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable label="借用数量" v-model="item.amount10"  placeholder="请填写借用数量！" @blur="validField('amount')" :error-message="message.amount"  />

                <span class="van-goods-span-number">#11</span>
              </van-cell-group>

              <van-cell-group v-show="size>=12" style="margin-top:10px;position:relative;border-top:0px solid #fefefe;">

                <!-- 物品名称（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable :label="item.type == '传屏设备' ? '传屏编号': '设备名称'" v-model="item.name11"  :placeholder="item.type == '传屏设备' ? '请填写传屏编号!': '请填写设备名称!'" @blur="validField('name')" :error-message="message.name"  />
                <!-- 借用数量（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable label="借用数量" v-model="item.amount11"  placeholder="请填写借用数量！" @blur="validField('amount')" :error-message="message.amount"  />

                <span class="van-goods-span-number">#12</span>
              </van-cell-group>

              <van-cell-group v-show="size>=13" style="margin-top:10px;position:relative;border-top:0px solid #fefefe;">

                <!-- 物品名称（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable :label="item.type == '传屏设备' ? '传屏编号': '设备名称'" v-model="item.name12"  :placeholder="item.type == '传屏设备' ? '请填写传屏编号!': '请填写设备名称!'" @blur="validField('name')" :error-message="message.name"  />
                <!-- 借用数量（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable label="借用数量" v-model="item.amount12"  placeholder="请填写借用数量！" @blur="validField('amount')" :error-message="message.amount"  />

                <span class="van-goods-span-number">#13</span>
              </van-cell-group>

              <van-cell-group v-show="size>=14" style="margin-top:10px;position:relative;border-top:0px solid #fefefe;">

                <!-- 物品名称（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable :label="item.type == '传屏设备' ? '传屏编号': '设备名称'" v-model="item.name13"  :placeholder="item.type == '传屏设备' ? '请填写传屏编号!': '请填写设备名称!'" @blur="validField('name')" :error-message="message.name"  />
                <!-- 借用数量（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable label="借用数量" v-model="item.amount13"  placeholder="请填写借用数量！" @blur="validField('amount')" :error-message="message.amount"  />

                <span class="van-goods-span-number">#14</span>
              </van-cell-group>

              <van-cell-group v-show="size>=15" style="margin-top:10px;position:relative;border-top:0px solid #fefefe;">

                <!-- 物品名称（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable :label="item.type == '传屏设备' ? '传屏编号': '设备名称'" v-model="item.name14"  :placeholder="item.type == '传屏设备' ? '请填写传屏编号!': '请填写设备名称!'" @blur="validField('name')" :error-message="message.name"  />
                <!-- 借用数量（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable label="借用数量" v-model="item.amount14"  placeholder="请填写借用数量！" @blur="validField('amount')" :error-message="message.amount"  />

                <span class="van-goods-span-number">#15</span>
              </van-cell-group>

               <van-cell-group v-show="size>=16" style="margin-top:10px;position:relative;border-top:0px solid #fefefe;">

                <!-- 物品名称（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable :label="item.type == '传屏设备' ? '传屏编号': '设备名称'" v-model="item.name15"  :placeholder="item.type == '传屏设备' ? '请填写传屏编号!': '请填写设备名称!'" @blur="validField('name')" :error-message="message.name"  />
                <!-- 借用数量（HR需要确认/修改） -->
                <van-field :readonly="readonly"  clearable label="借用数量" v-model="item.amount15"  placeholder="请填写借用数量！" @blur="validField('amount')" :error-message="message.amount"  />

                <span class="van-goods-span-number">#16</span>
              </van-cell-group>

              <van-cell-group style="margin-top:10px;">

                <van-cell value="人员信息" style="margin-left:0px;margin-left:-3px;font-size: 0.95rem;" />

                <!-- 借用人员（HR需要确认/修改） -->
                <van-field :readonly="readonly" required clearable label="借用人员" v-model="item.receive_name"  placeholder="请填写您的姓名！" @blur="validField('receive_name')" :error-message="message.receive_name"  />
                <!-- 单位名称（HR需要确认/修改） -->
                <van-field :readonly="readonly" required clearable label="单位名称" v-model="item.company" placeholder="请填写您的单位名称！" @blur="validField('company')" :error-message="message.company"/>
                <!-- 部门名称（HR需要确认/修改） -->
                <van-field :readonly="readonly" required clearable label="部门名称" v-model="item.department" placeholder="请填写您的部门名称！" @blur="validField('department')" :error-message="message.department" />

              </van-cell-group>

              <van-cell-group id="van-user-list" class="van-user-list" style="margin-top:10px;">
                <van-cell value="领用管理" style="margin-left:0px;margin-left:-3px;font-size: 0.95rem;" />
                <van-field required clearable label="接待人员" v-model="item.user_admin_name" placeholder="请输入领用接待人员!" @blur="querySealMan();" @click="querySealMan();" />
                <van-address-list v-show="userList.length > 0" v-model="userid" :list="userList" default-tag-text="默认" edit-disabled @select="selectSealUser()" />
              </van-cell-group>

              <van-cell-group style="margin-top:10px;">
                <van-cell value="备注说明" style="margin-left:0px;margin-left:-3px;font-size: 0.95rem;" />
                <!-- 备注说明（HR需要确认/修改） -->
                <van-field :readonly="readonly" :required="false" clearable label="备注说明" v-model="item.remark"  rows="2" autosize type="textarea"  maxlength="256"  placeholder="请填写备注说明信息，如相关流程，特殊事项及情况！" @blur="validField('remark')" :error-message="message.remark"  />
              </van-cell-group>

              <van-cell-group style="margin-top:10px;" v-show="processLogList.length > 0">
                <van-cell value="处理记录" style="margin-left:0px;margin-left:-3px;font-size: 0.95rem;" />
                <div>
                  <van-steps direction="vertical" :active="processLogList.length - 1">
                    <template v-for="value in processLogList">
                      <van-step :key="value.id">
                        <h3>{{ value.action + ' ' + value.employee + ' ' + value.action_opinion }}</h3>
                        <p>{{ value.create_time }}</p>
                      </van-step>
                    </template>
                  </van-steps>
                </div>
              </van-cell-group>

            </van-form>

          </van-cell-group>

          <div style="margin-top:30px;margin-left:0px;margin-right:10px;margin-bottom:10px;border-top:1px solid #efefef;" >
            <van-button color="linear-gradient(to right, #ff6034, #ee0a24)" type="primary" block @click="handleApply();" style="border-radius: 10px 10px 10px 10px; text-align: center;"  >提交</van-button>
          </div>

          <div style="height:500px;" ></div>
        </div>
      </div>

    </section>

    <setion v-if="!iswechat" >
      <div class="section-nowechat">
        请使用微信客户端打开
      </div>
    </setion>

  </div>
  </keep-alive>
</template>
<script>
import * as storage from '@/request/storage';
import * as tools from '@/request/tools';
import * as announce from '@/request/announce';
import * as task from '@/request/task';
import * as query from '@/request/query';
import * as constant from '@/request/constant';
import * as workflow from '@/request/workflow';
import * as manageAPI from '@/request/manage';
import * as wflowprocess from '@/request/wflow.process';
import * as workconfig from '@/request/workconfig';

export default {
    mixins: [window.mixin],
    data() {
        return {
            pageName: "详情",
            momentNewMsg: true,
            tabname: '1',
            active: 1,
            id:'',
            type:'',
            title:'奖罚申请表',
            content:'',
            files:'',
            from:'',
            tname:'',
            bname:'',
            tasktype:'done',
            previewurl:'',
            purl:'',
            tableInfo:'',
            orderInfo:'',
            status:'',
            status_type:'',
            fields:[],
            groupid:'group00',
            sealuserid:'',
            userid:'',
            userList:[],
            huserid:'',
            huserList:[],
            auserid:'',
            auserList:[],
            fuserid:'',
            fuserList:[],
            muserid:'',
            muserList:[],
            size:1,
            processLogList:[],
            iswechat:false,
            isfirst:true,
            dockFlag: false,
            uploadURL:'https://upload.yunwisdom.club:30443/sys/common/upload',
            message: workconfig.compValidation.entryjob.message,
            valid: workconfig.compValidation.entryjob.valid,
            item:{
              id: '',
              serialid:'',
              create_time: dayjs().format('YYYY-MM-DD'),
              create_by: '',
              receive_time: dayjs().format('YYYY-MM-DD'), //借用时间
              name:'', //借用物品名称
              amount:'',//借用数量
              name1:'', //借用物品名称
              amount1:'',//借用数量
              name2:'', //借用物品名称
              amount2:'',//借用数量
              name3:'', //借用物品名称
              amount3:'',//借用数量
              name4:'', //借用物品名称
              amount4:'',//借用数量
              name5:'', //借用物品名称
              amount5:'',//借用数量
              name6:'', //借用物品名称
              amount6:'',//借用数量
              name7:'', //借用物品名称
              amount7:'',//借用数量
              name8:'', //借用物品名称
              amount8:'',//借用数量
              name9:'', //借用物品名称
              amount9:'',//借用数量
              name10:'', //借用物品名称
              amount10:'',//借用数量
              name11:'', //借用物品名称
              amount11:'',//借用数量
              name12:'', //借用物品名称
              amount12:'',//借用数量
              name13:'', //借用物品名称
              amount13:'',//借用数量
              name14:'', //借用物品名称
              amount14:'',//借用数量
              name15:'', //借用物品名称
              amount15:'',//借用数量
              receive_name:'',//借用人员名称
              department:'',//借用部门名称
              remark:'',//备注说明
              type:'',//借用类别
              company:'',//单位名称
              approve_name:'',//借用审批人员
              workflow:'',//关联流程
              approve:'',//借用审批人员
              userid:'',
              user_group_ids:'',
              user_group_names:'',
              user_admin_name:'',
              status: '',
            },
            back:'/app',
            workflowlist:[],
            announces:[],
            informList:[],
            fileList:[],
            loading:false,
            officeList:[],
            tag:{
              showPicker: false,
              showPickerCommon: false,
              showPickerSealType: false,
              showPickerOrderType: false,
              showPickerJoinTime: false,
              showPickerDiploma: false,
            },
            searchFlag: false,
            dropMenuOldValue:'',
            dropMenuValue:'',
            dropMenuOption: [
              { text: '刷新', value: 2 , icon: 'replay' },
              { text: '重置', value: 4 , icon: 'aim' },
              { text: '应用', value: 5 , icon: 'apps-o' },
              { text: '首页', value: 6 , icon: 'wap-home-o' },
            ],
            statusType: workconfig.statusType,
            mailconfig: workconfig.mailconfig,
            config: workconfig.config,
            group: workconfig.group,
            currentKey:'',
            tablename:'bs_goods_borrow',
            readonly: false,
            goodstype: workconfig.goodstype,
            goodsborrowtype: workconfig.goodsborrowtype,
            diplomaType: workconfig.compcolumns.diplomaTypeColumns,
            acceptType: workconfig.compcolumns.acceptType,
            commonTypeColumns: workconfig.compcolumns.commonTypeColumns,
            sealTypeColumns: workconfig.compcolumns.sealTypeColumns,
        }
    },
    async activated() {
        this.$store.commit("toggleTipsStatus", -1);
        this.queryInfo();
    },
    async mounted() {
      this.queryInfo();
    },
    methods: {
      //点击显示或者隐藏菜单
      async headMenuToggle(){
        this.$refs.headMenuItem.toggle();
      },
      //点击顶部搜索
      async headMenuSearch(){
        if(this.searchWord){
          //刷新相应表单
          this.queryTabList(this.tabname);
          //显示搜索状态
          vant.Toast('搜索...');
          //等待一下
          await tools.sleep(300);
        }
        //显示刷新消息
        this.searchFlag = false;
      },
      //点击右侧菜单
      async headDropMenu(value){
        const val = this.dropMenuValue;
        switch (val) {
          case 2: //刷新数据
            this.dropMenuValue = this.dropMenuOldValue;
            await this.reduction();
            break;

          case 4: //重置数据
            this.dropMenuValue = this.dropMenuOldValue;
            await this.reduction();
            break;
          case 5: //返回应用
            this.$router.push(`/app`);
            break;
          case 6: //返回首页
            this.$router.push(`/explore`);
            break;
          default:
            console.log(`no operate. out of switch. `);
        }
      },
      //用户选择盖印人
      async querySealMan(){

        //获取盖章人信息
        const user_admin_name = this.item.user_admin_name;

        try {
          if(!!user_admin_name){

            //从用户表数据中获取填报人资料
            let user = await manageAPI.queryUserByNameHRM(user_admin_name.trim());

            if(!!user){

              //如果是用户数组列表，则展示列表，让用户自己选择
              if(Array.isArray(user)){

                try {
                  user.map((elem,index) => {
                    let company = elem.textfield1.split('||')[0];
                    company = company.slice(company.lastIndexOf('>')+1);
                    let department = elem.textfield1.split('||')[1];
                    department = department.slice(department.lastIndexOf('>')+1);
                    this.userList.push({id:elem.loginid , name:elem.lastname , tel:elem.mobile , address: company + "||" + elem.textfield1.split('||')[1] , company: company , department:department , mail: elem.email , isDefault: !index });
                  })

                  //获取盖印人姓名
                  this.item.user_admin_name = user[0].lastname;
                  //当前盖印人编号
                  this.item.userid = this.userid = user[0].loginid;

                } catch (error) {
                  console.log(error);
                }

              } else { //如果只有一个用户数据，则直接设置

                try {
                  let company = user.textfield1.split('||')[0];
                  company = company.slice(company.lastIndexOf('>')+1);
                  let department = user.textfield1.split('||')[1];
                  department = department.slice(department.lastIndexOf('>')+1);
                  //将用户数据推送至对方数组
                  this.userList.push({id:user.loginid , name:user.lastname , tel:user.mobile , address: company + "||" + user.textfield1.split('||')[1] , company: company , department:department , mail: this.item.dealMail, isDefault: !this.suserList.length });
                  //获取盖印人姓名
                  this.item.user_admin_name = user.lastname;
                  //当前盖印人编号
                  this.item.userid = this.userid = user.loginid;
                } catch (error) {
                  console.log(error);
                }

              }

              //遍历去重
              try {
                this.userList = this.userList.filter((item,index) => {
                  item.isDefault = index == 0 ? true : false;
                  let findex = this.userList.findIndex((subitem,index) => { return subitem.id == item.id });
                  return index == findex;
                })
              } catch (error) {
                console.log(error);
              }

            }
          }
        } catch (error) {
          console.log(error);
        }

      },
      //选中当前盖印人
      async selectSealUser(value){
        await tools.sleep(0);
        const id = this.userid;
        const user = this.userList.find((item,index) => {return id == item.id});
        //获取盖印人姓名
        this.item.user_admin_name = user.name;
        this.item.userid = id;
      },
      // 设置重置
      async reduction(){
        this.item = {
              id: '',
              serialid:'',
              create_time: dayjs().format('YYYY-MM-DD'),
              create_by: '',
              receive_time: dayjs().format('YYYY-MM-DD'), //借用时间
              name:'', //借用物品名称
              amount:'',//借用数量
              remark:'',//备注说明
              type:this.item.type,//借用类别
              approve_name:'',//借用审批人员
              workflow:'',//关联流程
              approve:'',//借用审批人员
              receive_name : this.item.receive_name , //借用人员名称
              department : this.item.department, //借用部门名称
              company : this.item.company, //单位名称
              status: '',
            };
      },
      /**
       * @function 获取处理日志
       */
      async queryProcessLog(){
        const id = tools.getUrlParam('id');
        try {
          this.processLogList = await workflow.queryPRLogHistoryByDataID(id);
          this.processLogList.map(item => { item.create_time = dayjs(item.create_time).format('YYYY-MM-DD HH:mm') });
          this.processLogList.sort();
        } catch (error) {
          console.log(error);
        }
      },

      //选中当前盖印人
      async selectFrontUser(value){
        await tools.sleep(0);
        const id = this.item.front_id;
        const user = this.fuserList.find((item,index) => {return id == item.id});
        //获取盖印人姓名
        this.item.front_name = user.name;
        this.item.front_id = id;
      },

      async validField(fieldName){
        //获取用户基础信息
        const userinfo = await storage.getStore('system_userinfo');

        // 邮箱验证正则表达式
        const regMail = workconfig.system.config.regexp.mail;

        this.message[fieldName] = tools.isNull(this.item[fieldName]) ? this.valid[fieldName] : '';

        if(fieldName.toLocaleLowerCase().includes('mail')) {
          this.message[fieldName] = regMail.test(this.item[fieldName]) ? '' : '请输入正确的邮箱地址！';
        }

        storage.setStore(`system_${this.tablename}_item#${this.item.type}#@${userinfo.realname}` , JSON.stringify(this.item) , 3600 * 2 );

        return tools.isNull(this.message[fieldName]);
      },

      afterRead(file) {

        file.status = 'uploading';
        file.message = '上传中...';

        setTimeout(() => {
          file.status = 'failed';
          file.message = '上传成功';
        }, 1000);
      },

      // 选择入职时间
      async joinTimeConfirm(value){
        this.item.join_time = dayjs(value).format('YYYY-MM-DD');
        this.validField('join_time');
        await tools.sleep(100);
        this.tag.showPickerJoinTime = false;
      },

      // 选择是否
      async commonTypeConfirm(value){
        this.item[this.currentKey] = value;
        this.validField(value);
        await tools.sleep(100);
        this.tag.showPickerCommon = false;
      },

      // 获取URL或者二维码信息
      async queryInfo() {

        try {
          //查询当前是否微信端
          this.iswechat = tools.isWechat();

          //查询上一页
          this.back = tools.getUrlParam('back') || '/app';

          //获取用户基础信息
          const userinfo = await storage.getStore('system_userinfo');

          //获取缓存信息
          const item = storage.getStore(`system_${this.tablename}_item#${this.item.type}#@${userinfo.realname}`);

          //根据URL参数查询物资类型
          this.item.type = this.goodsborrowtype[tools.getUrlParam('type')];

          //自动回显刚才填写的用户基础信息
          if(item){
            this.item.create_by = item.create_by || this.item.create_by;
            this.item.name = item.name || this.item.name;
            this.item.amount = item.amount || this.item.amount;
            this.item.receive_name = item.receive_name || userinfo.realname || this.item.receive_name ;
            this.item.department = item.department || this.item.department;
            this.item.remark = item.remark || this.item.remark;
            this.item.type = this.item.type || item.type || '办公用品';
            this.item.company = item.company || this.item.company;
            this.item.approve_name = item.approve_name || this.item.approve_name;
            this.item.workflow = item.workflow || this.item.workflow;
            this.item.approve = item.approve || this.item.approve;
            this.item.status = item.status || this.item.status;
          } else {
            this.item.receive_name = userinfo.realname || this.item.receive_name ;
            this.item.department = userinfo.department.name;
            this.item.company = userinfo.parent_company.name;
          }

        } catch (error) {
          console.log(error);
        }

      },

      // 用户提交入职登记表函数
      async handleApply() {

        //显示加载状态
        this.loading = true;

        //获取用户基础信息
        const userinfo = await storage.getStore('system_userinfo');

        //表单ID
        const id = tools.queryUniqueID();
        const type = tools.getUrlParam('type');

        //查询直接所在工作组
        const response = await query.queryRoleGroupList('COMMON_RECEIVE_BORROW' , this.item.userid);

        //获取到印章管理员组信息
        let user_group_ids = response && response.length > 0 ? response[0].userlist : '';
        let user_group_names = response && response.length > 0 ? response[0].enuserlist : '';

        //如果未获取用户名称，则直接设置用印人为分组成员
        if(tools.isNull(user_group_ids)){
          user_group_ids = this.item.userid;
          user_group_names = this.item.user_admin_name;
        }

        // 返回预览URL
        const receiveURL = encodeURIComponent(`${window.requestAPIConfig.vuechatdomain}/#/app/goodsview?id=${id}&statustype=office&type=${type}&role=front`);

        //第一步 保存用户数据到数据库中
        const elem = {
          id,
          create_time: dayjs().format('YYYY-MM-DD HH:mm:ss'),
          create_by : userinfo.username,
          name : this.item.name,
          amount : this.item.amount,
          receive_name:this.item.receive_name ,
          department : this.item.department,
          remark : this.item.remark,
          type : this.item.type,
          company : this.item.company,
          approve_name : this.item.approve_name,
          workflow : this.item.workflow,
          approve : this.item.approve,
          userid : this.item.userid,
          user_admin_name : this.item.user_admin_name,
          user_group_ids,
          user_group_names,
          pid: id,
          status: '待处理',
        }; // 待处理元素

        //第二步，向表单提交form对象数据
        const result = await manageAPI.postTableData(this.tablename , elem);

        //计算批量物品
        const tsize = this.size - 1;

        if(tsize >= 1){
          for(let i = 1; i <= tsize ; i++){

            let element = {
                  id: tools.queryUniqueID(),
                  create_time: dayjs().format('YYYY-MM-DD HH:mm:ss'),
                  create_by : userinfo.username,
                  name : this.item['name' + i],
                  amount : this.item['amount' + i],
                  receive_name:this.item.receive_name ,
                  department : this.item.department,
                  remark : this.item.remark,
                  type : this.item.type,
                  company : this.item.company,
                  approve_name : this.item.approve_name,
                  workflow : this.item.workflow,
                  approve : this.item.approve,
                  userid : this.item.userid,
                  user_admin_name : this.item.user_admin_name,
                  user_group_ids,
                  user_group_names,
                  pid: id,
                  status: '待处理',
                };
            //向表单提交form对象数据
            await manageAPI.postTableData(this.tablename , element);
          }
        }

        //发送自动设置排序号请求
        const patchResp = await superagent.get(workconfig.queryAPI.tableSerialAPI.replace('{table_name}', this.tablename)).set('accept', 'json');

         //查询数据
        const value = await query.queryTableData(this.tablename , id);

        //显示序列号
        this.item.serialid = value.serialid;

        //第三步 向HR推送入职引导通知，HR确认后，继续推送通知给行政、前台、食堂
        await superagent.get(`${window.requestAPIConfig.restapi}/api/v1/weappms/${user_group_ids},zhouxl0627,shur0411,wuzy0518,chenal0625,${userinfo.username}/奖罚申请登记通知：员工‘${userinfo.realname}(${userinfo.username})’ 部门:‘${userinfo.department.name}’ 单位:‘${userinfo.parent_company.name}’ 序号:‘${value.serialid}’ 奖罚申请登记完毕，请前台确认！?rurl=${receiveURL}`)
                .set('accept', 'json');


        /************************  工作流程日志(开始)  ************************/

        //查询直接所在工作组
        const resp = await query.queryRoleGroupList('COMMON_FRONT_ADMIN' , '');

        //获取后端配置前端管理员组
        const front = resp[0].userlist;
        const front_name = resp[0].enuserlist;

        //记录 审批人 经办人 审批表单 表单编号 记录编号 操作(同意/驳回) 意见 内容 表单数据
        const prLogHisNode = {
          id: tools.queryUniqueID(),
          table_name: this.tablename,
          main_value: id,
          proponents: userinfo.username,
          business_data_id : id ,//varchar(100)  null comment '业务数据主键值',
          business_code  : '000000000' ,//varchar(100)  null comment '业务编号',
          process_name   : '用印流程审批',//varchar(100)  null comment '流程名称',
          employee       : userinfo.realname ,//varchar(1000) null comment '操作职员',
          approve_user   : userinfo.username ,//varchar(100)  null comment '审批人员',
          action         : '发起'    ,//varchar(100)  null comment '操作动作',
          action_opinion : '发起借用申请[待处理]',//text          null comment '操作意见',
          operate_time   : dayjs().format('YYYY-MM-DD HH:mm:ss')   ,//datetime      null comment '操作时间',
          functions_station : userinfo.position,//varchar(100)  null comment '职能岗位',
          process_station   : '借用审批[奖罚申请]',//varchar(100)  null comment '流程岗位',
          business_data     : JSON.stringify(this.item),//text          null comment '业务数据',
          content           : `奖罚申请(${this.item.type}) ` + this.item.name + ' #经办人: ' + userinfo.username ,//text          null comment '业务内容',
          process_audit     : this.item.id + '##' + this.item.serialid ,//varchar(100)  null comment '流程编码',
          create_time       : dayjs().format('YYYY-MM-DD HH:mm:ss'),//datetime      null comment '创建日期',
          relate_data       : '',//text          null comment '关联数据',
          origin_data       : '',
        }

        await workflow.approveViewProcessLog(prLogHisNode);

        //同时推送一条待办记录给印章管理员

        //记录 审批人 经办人 审批表单 表单编号 记录编号 操作(同意/驳回) 意见 内容 表单数据
        const prLogNode = {
          id: tools.queryUniqueID(),
          table_name: this.tablename,
          main_value: id,
          proponents: front,
          business_data_id : id ,//varchar(100)  null comment '业务数据主键值',
          business_code  : '000000000' ,//varchar(100)  null comment '业务编号',
          process_name   : '用印流程审批',//varchar(100)  null comment '流程名称',
          employee       : front_name ,//varchar(1000) null comment '操作职员',
          approve_user   : front ,//varchar(100)  null comment '审批人员',
          action         : ''    ,//varchar(100)  null comment '操作动作',
          action_opinion : '审批借用申请',//text          null comment '操作意见',
          operate_time   : dayjs().format('YYYY-MM-DD HH:mm:ss')   ,//datetime      null comment '操作时间',
          functions_station : '前台',//varchar(100)  null comment '职能岗位',
          process_station   : '借用审批[奖罚申请]',//varchar(100)  null comment '流程岗位',
          business_data     : JSON.stringify(this.item),//text          null comment '业务数据',
          content           : `奖罚申请(${this.item.type}) ` + this.item.name + '#待处理 #经办人: ' + userinfo.username,//text          null comment '业务内容',
          process_audit     : this.item.id + '##' + this.item.serialid ,//varchar(100)  null comment '流程编码',
          create_time       : dayjs().format('YYYY-MM-DD HH:mm:ss'),//datetime      null comment '创建日期',
          relate_data       : '',//text          null comment '关联数据',
          origin_data       : '',
        }

        await workflow.taskViewProcessLog(prLogNode);

        /************************  工作流程日志(结束)  ************************/

        //设置状态
        this.loading = false;
        this.status = elem.status;
        this.readonly = true;

        //弹出确认提示
        await vant.Dialog.alert({
            title: '温馨提示',
            message: '已经向前台客户推送奖罚申请申请！',
          });

      }
    }
}
</script>
<style>
</style>
<style scoped>
    @import "../../assets/css/explore.css";
    @import "../../assets/css/sealinfo.css";
    @import "../../assets/css/goodsreceive.css";
</style>
