<template>
  <div class="offer-manage-wrapper">

    <div style="width: 100%;height: 26px;"></div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>考试信息管理</el-breadcrumb-item>
      <el-breadcrumb-item>考试信息</el-breadcrumb-item>
      <el-breadcrumb-item>添加考试</el-breadcrumb-item>
    </el-breadcrumb>
    <!--    <div class="offer-manage-card offer-b-card">-->
    <el-card class="box-card" >
<!--    <el-form ref="form" class="forum" :model="examTipsInputVo" label-width="80px">-->
      <el-form :model="examTipsInputVo">
      <el-form-item label="考试标题">
        <el-input v-model="examTipsInputVo.examTipsTitle"></el-input>
      </el-form-item>
      <el-form-item label="报名入口">
        <el-input v-model="examTipsInputVo.examUrl"></el-input>
      </el-form-item>
      <el-form-item label="考试类目">
        <el-select v-model="examTipsInputVo.examTypeId" placeholder="请选择考试类目">
          <el-option label="全国计算机等级考试" value="1"></el-option>
          <el-option label="计算机技术与软件专业技术资格（水平）考试" value="2"></el-option>
          <el-option label="CET大学英语考试" value="3"></el-option>
          <el-option label="全国会计从业资格考试" value="4"></el-option>
          <el-option label="中小学教师资格考试" value="5"></el-option>
          <el-option label="全国英语等级考试(PETS)" value="6"></el-option>
          <el-option label="研究生考试" value="7"></el-option>
          <el-option label="剑桥商务英语证书考试 (BEC)" value="8"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="报名时间">
        <el-col :span="11">
          <el-date-picker
            v-model="examTipsInputVo.value1"
            type="daterange"
            align="right"
            unlink-panels
            range-separator="至"
            start-placeholder="开始日期"
            end-placeholder="结束日期">
<!--            @input="testClickSignup"-->

          </el-date-picker>
        </el-col>
      </el-form-item>
      <el-form-item label="考试时间">
        <el-col :span="11">
          <el-date-picker
            v-model="examTipsInputVo.value2"
            type="daterange"
            align="right"
            unlink-panels
            range-separator="至"
            start-placeholder="开始日期"
            end-placeholder="结束日期"   >
<!--            @input="testClickExam"-->

          </el-date-picker>
        </el-col>
      </el-form-item>

      <el-form-item label="考试详情">
        <el-input type="textarea" v-model="examTipsInputVo.examComment"></el-input>
      </el-form-item>
      <el-form-item label="发布">
        <el-switch v-model="examTipsInputVo.deleted"></el-switch>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">立即创建</el-button>
        <el-button>取消</el-button>
      </el-form-item>
      </el-form>
    </el-card>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        examTipsInputVo: {
          value2:'',
          value1:'',
          examTipsTitle: '',
          examUrl:'',
          examTypeId: '',
          signupBegintime: '',
          signupEndtime: '',
          examBegintime: '',
          examEndtime: '',
          deleted: false,
          examComment: '',
          examScorestime:''
        }
      }
    },
    methods: {
      onSubmit: function () {
        // let examTipsInputVo = new FormData();
        // for (var key in this.formData) {
        //   examTipsInputVo.append(key, this.formData[key]);
        // }
        // console.log( examTipsInputVo.keys().next());
        let inputVo = this.examTipsInputVo;
        inputVo.signupBegintime = inputVo.value1[0];
        inputVo.signupEndtime = inputVo.value1[1];
        inputVo.examBegintime = inputVo.value2[0];
        inputVo.examEndtime = inputVo.value2[1];


        this.$axios({
          method: "post",
          url: "/examTips/editExamTips",
          headers: {
            //"Content-Type": "multipart/form-data"
            'Content-Type': 'application/json;charset=UTF-8'
          },
          withCredentials: true,
          data: JSON.stringify(inputVo)
        }).then((res) => {
          console.log(res);
        });
      }
    }
  }
</script>
<style>
  .forum {
    margin: 80px;
    width: 800px;
    padding: 35px 35px 15px 35px;
    background: #fff;
    border: 1px solid #eaeaea;
    -webkit-box-shadow: 0 0 25px #cac6c6;
    box-shadow: 0 0 25px #cac6c6;
  }
  .el-form{
    padding: 35px 35px 15px 35px;
  }
  .el-input {
    position: relative;
    font-size: 14px;
    /* display: inline-block; */
    width: 80%;
  }
  .el-select {
    /* display: inline-block; */
    /* position: relative; */
    width: 500px;
  }
  .el-form-item {
    margin-bottom: 22px;
    width: 60%;
  }
  .el-textarea {
    position: relative;
    display: inline-block;
    width: 100%;
    vertical-align: bottom;
    font-size: 14px;
    height: 100px;
  }
  .el-textarea__inner {
    height: 100px;
  }
</style>
