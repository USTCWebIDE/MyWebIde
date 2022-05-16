/* eslint-disable no-unused-expressions */
<template>
    <div class="mod-home">
      <h3>Mannge Your Projects</h3>
      <el-card class="maincard">
        <h3 style=" color: #fff;">Project Mannge</h3>
        <div>
          <ul class="listrencentused">
           
            <!-- <li class="rencentused"> 
                  
                     <div style="width:50px;height:50px;margin: auto;margin-top: 35%; " >
                      <el-tooltip class="item" effect="dark" content="Create New Sandbox" placement="top">
                        <img src="../../assets/img/add.png" width="50px" height="50px"    >
                      </el-tooltip>
                     </div>
                 
            </li> -->
            <li class="rencentused" v-for="item in projectList" @click="projectDisplay(item)">
              <div style="height: 120px;">
                <img src="../../assets/img/emptytemplate.png" width="200px" height="130px" v-if="item.projectType==0">
                <img src="../../assets/img/vuetemplate.png" width="200px"   height="130px" v-if="item.projectType==1">
                <img src="../../assets/img/reacttemplate.png" width="200px" height="130px" v-if="item.projectType==2">
                <img src="../../assets/img/angulartemplate.png" width="200px" height="130px" v-if="item.projectType==3">
                <img src="../../assets/img/statictemplate.png" width="200px" height="130px" v-if="item.projectType==4">
                <img src="../../assets/img/vanillatemplate.png" width="200px" height="130px" v-if="item.projectType==5">
              </div>
              <el-tooltip class="item" effect="dark" content="click to display project detail info" placement="top">
              <div  style="height: 80px; color:#fff;">
                  <div style="margin-top: 25px;margin-left: 10px;" v-if="item.projectType==0">Empty Project: <span>{{item.projectName}}</span></div>
                  <div style="margin-top: 25px;margin-left: 10px;" v-if="item.projectType==1">Vue: <span>{{item.projectName}}</span></div>
                  <div style="margin-top: 25px;margin-left: 10px;" v-if="item.projectType==2">React: <span>{{item.projectName}}</span></div>
                  <div style="margin-top: 25px;margin-left: 10px;" v-if="item.projectType==3">Angular: <span>{{item.projectName}}</span></div>
                  <div style="margin-top: 25px;margin-left: 10px;" v-if="item.projectType==4">static: <span>{{item.projectName}}</span></div>
                  <div style="margin-top: 25px;margin-left: 10px;" v-if="item.projectType==5">Vanilla: <span>{{item.projectName}}</span></div>
                  <div style="padding-top: 10px;padding-left: 10px;">
                    created:
                      {{item.createTime}}
                  </div>
              </div>
              </el-tooltip>
              </li>
  
            <!-- </li>
            <li class="rencentused" v-for="item in projectList">
              <div style="height: 120px;">
                <img src="../../assets/img/vuetemplate.png" width="200px" height="130px">
              </div>
              <div  style="height: 80px; color:#fff;">
                  <div style="margin-top: 25px;margin-left: 10px;">Vue</div>
                  <div style="padding-top: 10px;padding-left: 10px;"> <icon-svg name="yanjing" style="padding-top: 3px;" ></icon-svg>    701.1k codebox</div>
              </div>
            </li> -->
            <!-- <li class="rencentused">
              <div style="height: 120px;">
                <img src="../../assets/img/reacttemplate.png" width="200px" height="130px">
              </div>
              <div  style="height: 80px; color:#fff;">
                  <div style="margin-top: 25px;margin-left: 10px;">React</div>
                  <div style="padding-top: 10px;padding-left: 10px;"> <icon-svg name="yanjing" style="padding-top: 3px;" ></icon-svg>    531.1k codebox</div>
              </div>
            </li> -->
           
             
          </ul>
        </div>
      </el-card>
      <el-dialog
      title="Project details"
      :visible.sync="peojectDialogVisable"
      width="30%"
      :modal="false"
      left>
      <el-form :model="openProjectItem" label-position="left" label-width="100px">
        <el-form-item label="projectName"><el-input v-model="openProjectItem.projectName"  style="width: 200px;" disabled></el-input></el-form-item>
        <el-form-item label="projectType">
          <el-tag :type="displayType[openProjectItem.projectType]">{{type[openProjectItem.projectType]}}</el-tag>
        </el-form-item>
  
        <el-form-item :inline="true" label="创建时间">
          <el-col :span="15" >
            <el-input v-model="openProjectItem.createTime" disabled></el-input>
          </el-col>
        </el-form-item>
        <el-form-item :inline="true" label="上次修改时间">
          <el-col :span="15" >
            <el-input v-model="openProjectItem.lastEiditTime" disabled></el-input>
          </el-col>
        </el-form-item>
   
      </el-form>
      <div slot="footer" class="dialog-footer">
       
        <el-button @click="comfirmVisable = true" type="danger">删除项目</el-button>
        <el-button @click="peojectDialogVisable = false">返 回</el-button>
      </div>
      </el-dialog>
      <el-dialog
      title="警告:"
      :visible.sync="comfirmVisable"
      width="30%"
      :modal="false"
      left>
      <span>{{'确定要删除项目'+openProjectItem.projectName+'吗?'}}</span>
      <div slot="footer" class="dialog-footer">
        <el-button @click="submitDeleteProject">确 定</el-button>
        <el-button @click="comfirmVisable = false">返 回</el-button>
      </div>
      </el-dialog>
    </div>
  </template>
  
  <script>
    export default {
      data () {
        return {
          projectList: [],
          peojectDialogVisable: false,
          comfirmVisable: false,
          openProjectItem: '',
          type: [
            'Empty Project',
            'Vue',
            'React',
            'Angular',
            'static',
            'Vanilla'
          ],
          displayType: ['', 'success', 'warning', 'info', 'danger', '']
        }
      },
      methods: {
        submitDeleteProject () {
          this.peojectDialogVisable = false
          this.comfirmVisable = false
          this.$http({
            url: this.$http.adornUrl('/ideproject/projecttable/deleteProjectAndFile'),
            method: 'post',
            data: this.openProjectItem
          }).then(({ data }) => {
            if (data && data.code == 0) {
              this.$message.success('项目删除成功！')
              window.sessionStorage.removeItem("currentEiditProjectId")
              this.peojectDialogVisable = false
              this.comfirmVisable = false
              this.getProjectList()
            } else this.$message.error(data.msg)
          })
    },
        projectDisplay (data) {
          this.openProjectItem = data
          this.peojectDialogVisable = true
        },
        getProjectList () {
          let uid = window.sessionStorage.getItem('loginUserId')
          if (uid == null) return this.$message.error('出现未知错误！')
          this.$http({
            url: this.$http.adornUrl('/ideproject/projecttable/getProjectTableByUid?userId=' + uid),
            method: 'get'
          }).then(({ data }) => {
            if (data && data.code == 0) {
              this.projectList = data.projectList
            } else this.$message.error(data.msg)
          })
        }
      },
      activated () {
        this.getProjectList()
      }
    }
  </script>
  
  <style scoped>
    .maincard{
      background-color: rgb(21,21,21);
      
    }
    .listrencentused{
      display: flex;
      justify-content: start;
      flex-wrap:wrap;
    }
    .mod-home {
      line-height: 1.5;
    }
    .rencentused{
      cursor: pointer;
      margin: 10px;
      list-style: none;
      width: 200px;
      height: 200px;
      border: 1px solid;
      border-radius: 3px;
    }
    .rencentused:hover{
      box-shadow: 10px 10px 15px #888888
    }
  </style>
  
  