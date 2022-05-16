/* eslint-disable no-unused-vars */
/* eslint-disable no-unused-vars */
/* eslint-disable eqeqeq */
<template>
  <div class="mod-home">
    <h3>Home</h3>
    <el-card class="maincard">
      <h3 style=" color: #fff;">Recently Eidit Project</h3>
      <div>
        <ul class="listrencentused">
         
          <li class="rencentused" @click="startNewProject"> 
            <div style="width:50px;height:50px;margin: auto;margin-top: 35%; " >
            <el-tooltip class="item" effect="dark" content="Create New Project" placement="top">
              <img src="../../assets/img/add.png" width="50px" height="50px"    >
            </el-tooltip>
            </div>
          </li>
       
          
          <li class="rencentused" v-for="item in projectList" @click="chooseRencentusedProject(item)">
            <div style="height: 120px;">
              <img src="../../assets/img/emptytemplate.png" width="200px" height="130px" v-if="item.projectType==0">
              <img src="../../assets/img/vuetemplate.png" width="200px"   height="130px" v-if="item.projectType==1">
              <img src="../../assets/img/reacttemplate.png" width="200px" height="130px" v-if="item.projectType==2">
              
              <img src="../../assets/img/angulartemplate.png" width="200px" height="130px" v-if="item.projectType==3">
              <img src="../../assets/img/statictemplate.png" width="200px" height="130px" v-if="item.projectType==4">
              <img src="../../assets/img/vanillatemplate.png" width="200px" height="130px" v-if="item.projectType==5">
            </div>
            <el-tooltip class="item" effect="dark" content="click to coding project" placement="top">
            <div  style="height: 80px; color:#fff;">
                <div style="margin-top: 25px;margin-left: 10px;" v-if="item.projectType==0">Empty Project: <span>{{item.projectName}}</span></div>
                <div style="margin-top: 25px;margin-left: 10px;" v-if="item.projectType==1">Vue: <span>{{item.projectName}}</span></div>
                <div style="margin-top: 25px;margin-left: 10px;" v-if="item.projectType==2">React: <span>{{item.projectName}}</span></div>
                <div style="margin-top: 25px;margin-left: 10px;" v-if="item.projectType==3">Angular: <span>{{item.projectName}}</span></div>
                <div style="margin-top: 25px;margin-left: 10px;" v-if="item.projectType==4">static: <span>{{item.projectName}}</span></div>
                <div style="margin-top: 25px;margin-left: 10px;" v-if="item.projectType==5">Vanilla: <span>{{item.projectName}}</span></div>
                <div style="padding-top: 10px;padding-left: 10px;">
                   <icon-svg name="yanjing" style="padding-top: 3px;" ></icon-svg>  
                    {{item.lastEiditTime}}
                </div>
            </div>
            </el-tooltip>
            

          </li>

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
      :visible.sync="chooseTemplateDialogVisible"
      title="Create Project In MyWebIDE"
      width="50%"
      class="selectTemplateDiv"
      >
      <div style="height: 30px;width:160px;position:absolute;margin-top: -68px;margin-left: 450px;">
        <el-input
        size="mini"
        placeholder="搜索模板"
        prefix-icon="el-icon-search"
        v-model="searchData">
      </el-input>
      </div>
      <div style="height: 300px;overflow-y:scroll;">
        <div style="height: 20px;">
          Recently Used Templates
        </div>
        <div>
          <ul style="display: flex;justify-content: space-between;flex-wrap: wrap;padding: 10px">
            <li class="templateschoose" v-for="item in recentProjectTemplates" @click="createNewProject(item)">
              <div style="display: flex;justify-content: start;padding-left: 5px;">
                <div style="width: 40px;height: 38px;">
                  <icon-svg v-if="item.templateId==0" name="nullproject" class="site-sidebar__menu-icon" style="width: 40px;height: 37px;"> </icon-svg>
                  <icon-svg v-if="item.templateId==1" name="vue" class="site-sidebar__menu-icon" style="width: 40px;height: 40px;"> </icon-svg>
                  <icon-svg v-if="item.templateId==2" name="React" class="site-sidebar__menu-icon" style="width: 40px;height: 40px;"></icon-svg>
                  <icon-svg v-if="item.templateId==3" name="Angular" class="site-sidebar__menu-icon" style="width: 40px;height: 40px;"></icon-svg>
                  <icon-svg v-if="item.templateId==4" name="static" class="site-sidebar__menu-icon" style="width: 40px;height: 40px;"></icon-svg>
                  <icon-svg v-if="item.templateId==5" name="Vanilla" class="site-sidebar__menu-icon" style="width: 40px;height: 40px;"></icon-svg>
               
                </div>
                <div style="width: 180px;height: 40px;line-height: 40px;text-align: left;padding-left: 5px;">
                  <div v-if="item.templateId==0" style="font-size: larger;margin-top: -10px;">Empty Project</div>
                  <div v-if="item.templateId==1" style="font-size: larger;margin-top: -10px;">Vue</div>
                  <div v-if="item.templateId==2" style="font-size: larger;margin-top: -10px;">React</div>
                  <div v-if="item.templateId==3" style="font-size: larger;margin-top: -10px;">Angular</div>
                  <div v-if="item.templateId==4" style="font-size: larger;margin-top: -10px;">static</div>
                  <div v-if="item.templateId==5" style="font-size: larger;margin-top: -10px;">Vanilla</div>
                  <div style="font-size: 10px;margin-top: -23px;">by MyWebIDE</div>
                </div>
              </div>
            </li>
            <!-- <li class="templateschoose">
              <div style="display: flex;justify-content: start;padding-left: 5px;">
                <div style="width: 40px;height: 38px;">
                  <icon-svg name="React" class="site-sidebar__menu-icon" style="width: 40px;height: 40px;">
                  </icon-svg>
                </div>
                <div style="width: 80px;height: 40px;line-height: 40px;text-align: left;padding-left: 5px;">
                  <div style="font-size: larger;margin-top: -10px;">React</div>
                  <div style="font-size: 10px;margin-top: -23px;">by MyWebIDE</div>
                </div>
              </div>
            </li> -->
          </ul>
        </div>
        <div style="height: 20px;margin-top: -20px;">
          MyWebIDE Templates
        </div>
        <div>
          <ul style="display: flex;justify-content: space-between;flex-wrap: wrap;padding: 10px">
           
            <li class="templateschoose" v-for="item in ideProjectTemplates" @click="createNewProject(item)">
              <div style="display: flex;justify-content: start;padding-left: 5px;">
                <div style="width: 40px;height: 38px;">
                    <icon-svg v-if="item.templateId==0" name="nullproject" class="site-sidebar__menu-icon" style="width: 40px;height: 37px;"></icon-svg>
                    <icon-svg v-if="item.templateId==1" name="vue" class="site-sidebar__menu-icon" style="width: 40px;height: 40px;"></icon-svg>
                    <icon-svg v-if="item.templateId==2" name="React" class="site-sidebar__menu-icon" style="width: 40px;height: 40px;"></icon-svg>
                    <icon-svg v-if="item.templateId==3" name="Angular" class="site-sidebar__menu-icon" style="width: 40px;height: 40px;"></icon-svg>
                    <icon-svg v-if="item.templateId==4" name="static" class="site-sidebar__menu-icon" style="width: 40px;height: 40px;"></icon-svg>
                    <icon-svg v-if="item.templateId==5" name="Vanilla" class="site-sidebar__menu-icon" style="width: 40px;height: 40px;"> </icon-svg>
                </div>
                <div style="width: 160px;height: 40px;line-height: 40px;text-align: left;padding-left: 5px;">
                  <div style="font-size: larger;margin-top: -10px;">{{item.name}}</div>
                  <div style="font-size: 10px;margin-top: -23px;">by MyWebIDE</div>
                </div>
              </div>
            </li>

            <!-- <li class="templateschoose">
              <div style="display: flex;justify-content: start;padding-left: 5px;">
                <div style="width: 40px;height: 38px;">
                  <icon-svg name="vue" class="site-sidebar__menu-icon" style="width: 40px;height: 40px;">
                  </icon-svg>
                </div>
                <div style="width: 160px;height: 40px;line-height: 40px;text-align: left;padding-left: 5px;">
                  <div style="font-size: larger;margin-top: -10px;">Vue</div>
                  <div style="font-size: 10px;margin-top: -23px;">by MyWebIDE</div>
                  </div>
              </div>
            </li>
            <li class="templateschoose">
              <div style="display: flex;justify-content: start;padding-left: 5px;">
                <div style="width: 40px;height: 38px;">
                  <icon-svg name="Vanilla" class="site-sidebar__menu-icon" style="width: 40px;height: 40px;">
                  </icon-svg>
                </div>
                <div style="width: 160px;height: 40px;line-height: 40px;text-align: left;padding-left: 5px;">
                  <div style="font-size: larger;margin-top: -10px;">React</div>
                  <div style="font-size: 10px;margin-top: -23px;">by MyWebIDE</div>
                </div>
              </div>
            </li>
            <li class="templateschoose">
              <div style="display: flex;justify-content: start;padding-left: 5px;">
                <div style="width: 40px;height: 38px;">
                  <icon-svg name="Angular" class="site-sidebar__menu-icon" style="width: 40px;height: 40px;">
                  </icon-svg></div>
                  <div style="width: 160px;height: 40px;line-height: 40px;text-align: left;padding-left: 5px;">
                    <div style="font-size: larger;margin-top: -10px;">Angular</div>
                    <div style="font-size: 10px;margin-top: -23px;">by MyWebIDE</div>
                    </div>
              </div>
            </li>
            <li class="templateschoose">
              <div style="display: flex;justify-content: start;padding-left: 5px;">
                <div style="width: 40px;height: 38px">
                  <icon-svg name="static" class="site-sidebar__menu-icon" style="width: 40px;height: 40px;">
                  </icon-svg>
                </div>
                <div style="width: 160px;height: 40px;line-height: 40px;text-align: left;padding-left: 5px;">
                <div style="font-size: larger;margin-top: -10px;">static</div>
                <div style="font-size: 10px;margin-top: -23px;">by MyWebIDE</div>
                </div>
              </div>
            </li>
            <li class="templateschoose">
              <div style="display: flex;justify-content: start;padding-left: 5px;">
                <div style="width: 40px;height: 38px">
                  <icon-svg name="vue" class="site-sidebar__menu-icon" style="width: 40px;height: 40px;">
                  </icon-svg>
                </div>
                <div style="width: 160px;height: 40px;line-height: 40px;text-align: left;padding-left: 5px;">
                  <div style="font-size: larger;margin-top: -10px;">Vue3</div>
                  <div style="font-size: 10px;margin-top: -23px;">by MyWebIDE</div>
                  </div>
              </div>
            </li>
          -->
          </ul>
        </div>
      </div>
    </el-dialog>
    <el-dialog title="New Project" :visible.sync="newProjectVisable"
    :modal="false"
    width="30%"
    >
    <el-form :model="newProject" label-position="left" label-width="80px"  
    
    @keyup.enter.native="submitnewProject">
      <el-form-item label="项目名" >
        <el-input v-model="newProject.projectName" placeholder="项目名'"  >
        </el-input></el-form-item>
    </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="newProjectVisable = false" >取 消</el-button>
        <el-button @click="submitnewProject">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
  import Vue from 'vue'
  export default {
    data () {
      return {
        projectList: [],
        loading: '',
        chooseTemplateDialogVisible: false,
        newProjectVisable: false,
        newProject: {
          userId: '',
          projectName: '',
          projectId: '',
          projectType: '',
          createTime: '',
          lastEiditTime: '',
          templateId: ''
        },
        searchData: '',
        recentProjectTemplates: [],
        ideProjectTemplates: []
  
      }
    },
    created () {
      this.getProjectList()
    },
    activated () {
      this.getProjectList()
    },
    methods: {
      startLoading () {
        this.loading = Vue.prototype.$loading({
          lock: true,
          text: '努力加载中...',
          background: 'rgba(255,255,255,.4)',
          target: document.querySelector('.selectTemplateDiv') // 设置加载动画区域
        })
      },
      endLoading () {
        this.loading.close()
      },
      submitnewProject () {
        this.startLoading()
        this.newProject.userId = window.sessionStorage.getItem('loginUserId')
        // console.log(this.newProject.projectName)
        if (this.newProject.projectName !== '') {
          // console.log(this.newProject)
          this.$http({
            url: this.$http.adornUrl('/ideproject/projecttable/createNewProject'),
            method: 'post',
            data: this.newProject
          }).then(({ data }) => {
            if (data && data.code == 0) {
              this.newProjectVisable = false
              let temp = data.projectTable
             // console.log(temp)
              this.$http({
                url: this.$http.adornUrl('/ideproject/filetable/createFileByTemplate'),
                method: 'post',
                data: temp
              }).then(({ data }) => {
                if (data && data.code == 0) {
                  window.sessionStorage.setItem('currentEiditProjectId', temp.projectId)
                  this.$router.push({ name: 'project' })
                } else this.$message.error('系统繁忙！')
              })
            } else {
              this.$message.error(data.msg)
            }
          })
        } else {
          this.$message.error('项目名不能为空！')
        }
        this.endLoading()
        this.chooseTemplateDialogVisible = false
      },
      createNewProject (data) {
        // console.log(data)
        this.newProjectVisable = true
        this.newProject.projectType = data.templateId
        this.newProject.templateId = data.templateId
        // this.$http({
        //   url: this.$http.adornUrl('/'),
        //   method: 'post',
        //   data: this.$http.adornData(data, false)
        // }).then(({ data }) => { })
      },
      startNewProject () {
        // console.log('newproject')
        this.getRecentProjectTemplate()
        this.getIdeProjectTemplates()
        this.chooseTemplateDialogVisible = true
      },
      getIdeProjectTemplates () {
        this.$http({
          url: this.$http.adornUrl('/ideproject/templatetable/list'),
          method: 'get',
          params: this.$http.adornParams({})
        }).then(({ data }) => {
          if (data && data.code == 0) {
            this.ideProjectTemplates = data.page.list
          }
        })
      },
      getRecentProjectTemplate () {
        let uid = window.sessionStorage.getItem('loginUserId')
        if (uid != null) {
          this.$http({
            url: this.$http.adornUrl('/ideproject/projecttable/getRecentTemplateByUid?userId=' + uid),
            method: 'post'
          }).then(({ data }) => {
            if (data && data.code == 0) {
              this.recentProjectTemplates = data.recentProjectList
            } else this.$message.error('系统繁忙！')
          })
        }
      },
      getProjectList () {
        let uid = window.sessionStorage.getItem('loginUserId')
        this.$http({
          url: this.$http.adornUrl('/ideproject/projecttable/getProjectListByUserId'),
          method: 'post',
          data: {
            'userId': uid
          }
        }).then(({ data }) => {
          if (data && data.code == 0) {
            this.projectList = data.projectList
          } else {
            this.$message.error('系统繁忙！')
          }
        })
      },
      chooseRencentusedProject (data) {
        window.sessionStorage.setItem('currentEiditProjectId', data.projectId)
        this.$router.push({ name: 'project' })
      }
    }
  }
</script>

<style scoped>
.templateschoose{
  list-style: none;
  width: 300px;height: 40px;
  margin-bottom: 20px;
  border-bottom: 1px solid black;
  border-radius: 3px;
  background-color: honeydew;
 
}
.templateschoose:hover{
  background-color: white;
  color: black;
  cursor: pointer;
}
.maincard{
    background-color: rgb(21,21,21);
    
  }
  .listrencentused{
    display: flex;
    /* justify-content: space-around; */
    flex-wrap:wrap;
  }
  .rencentused:after{
    content: "";
    width: 32%;
  }
  .mod-home {
    line-height: 1.5;
  }
  .rencentused{
    cursor: pointer;
    margin: 20px;
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

