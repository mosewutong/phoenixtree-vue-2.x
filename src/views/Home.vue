<template>
  <div class="home">
    <phoenixtree-header
      :dropdowmArr="dropdowmArr"
      :bcColor="'linear-gradient(194deg, #3d7ffc 0%, #0450ff 100%);'"
      @emitChange="commandChange"
    >
      <template v-slot:header-title>
        <img src="../assets/mo.png" alt="">
        cesh
      </template>

      <!-- <template v-slot:header-btn-pic>{{userPic}}</template> -->
      <!-- <template v-slot:header-btn-name>{{userName}}</template> -->
    </phoenixtree-header>

    <div class="homeContent">
      <phoenixtree-layout
        :noPadding='false'
        :noContentPadding="false"
        :minWidth="'1000px'"
      >
        <template v-slot:Frame-content>
          <phoenixtree-base-table
            :pageTotal="200"
            :pageSize="20"
            :currentPage="4"
            @currentChange="currentChange"
          >
            <template v-slot:slot-title>标题文字区</template>

            <template v-slot:slot-title-btn>
              <el-button>标题按钮区</el-button>
            </template>

            <template v-slot:slot-form>
              <el-form
                :inline="true"
                :model="formInline"
              >
                <el-form-item label="审批人">
                  <!-- <el-input v-model="formInline.user" placeholder="审批人" size="mini"></el-input> -->
                  <!-- <el-radio v-model="formInline.user" label="1">备选项</el-radio> -->
                  <el-checkbox v-model="formInline.user">备选项</el-checkbox>
                </el-form-item>
                <el-form-item label="审批人">
                  <el-select v-model="formInline.user" placeholder="请选择">
                    <el-option
                      v-for="item in options"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value">
                    </el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="审批人">
                  <el-input v-model="formInline.user" placeholder="审批人" size="small"></el-input>
                </el-form-item>
                <el-form-item label="审批人">
                  <el-date-picker
                    v-model="formInline.value1"
                    type="datetimerange"
                    range-separator="至"
                    start-placeholder="开始日期"
                    end-placeholder="结束日期">
                  </el-date-picker>
                </el-form-item>
                <el-form-item label="日期">
                  <el-date-picker
                    v-model="formInline.user"
                    type="date"
                    placeholder="选择日期">
                  </el-date-picker>
                </el-form-item>
              </el-form>
            </template>

            <template v-slot:slot-form-btn>
              <el-button>查询</el-button>
            </template>

            <template v-slot:slot-mid-btn>
              <el-button type="primary">查询1</el-button>
              <el-button>查询2</el-button>
            </template>
          </phoenixtree-base-table>
        </template>
      </phoenixtree-layout>
    </div>
  </div>
  
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      formInline: {},
      userPic: '',
      userName: '',
      dropdowmArr: [
        {
          name: '登出',
          command: 'out',
        },
        {
          name: '修改',
          command: 'change',
        }
      ],
      options: [{
          value: '选项1',
          label: '黄金糕'
        }, {
          value: '选项2',
          label: '双皮奶'
        }, {
          value: '选项3',
          label: '蚵仔煎'
        }, {
          value: '选项4',
          label: '龙须面'
        }, {
          value: '选项5',
          label: '北京烤鸭'
        }],
    }
  },
  mounted(){
    // this.userName = 'administrator';
    this.userName = '墨色梧桐';
    this.userPic = this.userName.substr(0,1);
    this.userPic = this.userPic.toUpperCase();
  },
  methods:{
    commandChange(obj){
      console.log('1==',obj);
    },
    currentChange(num){
      console.log('页码',num)
    }
  }
}
</script>

<style lang="less" scoped>
.home{
  width: 100%;
  height: 100%;
  .homeContent{
    width: 100%;
    height: calc(100% - 50px);
  }
}
</style>
