<template>
    <div class="mod-home">
      <h3>Templates</h3>
      <el-card class="maincard">
        <h3 style=" color: #fff;">Templates Available</h3>
        <div>
          <ul class="listrencentused">
           
            <!-- <li class="rencentused"> 
                  
                     <div style="width:50px;height:50px;margin: auto;margin-top: 35%; " >
                      <el-tooltip class="item" effect="dark" content="Create New Sandbox" placement="top">
                        <img src="../../assets/img/add.png" width="50px" height="50px"    >
                      </el-tooltip>
                     </div>
                 
            </li> -->
            <li class="rencentused" v-for="item in ideProjectTemplates">
              <div style="height: 120px;">
                <img src="../../assets/img/emptytemplate.png" width="200px" height="130px" v-if="item.templateId==0">
                <img src="../../assets/img/vuetemplate.png" width="200px"   height="130px" v-if="item.templateId==1">
                <img src="../../assets/img/reacttemplate.png" width="200px" height="130px" v-if="item.templateId==2">
                <img src="../../assets/img/angulartemplate.png" width="200px" height="130px" v-if="item.templateId==3">
                <img src="../../assets/img/statictemplate.png" width="200px" height="130px" v-if="item.templateId==4">
                <img src="../../assets/img/vanillatemplate.png" width="200px" height="130px" v-if="item.templateId==5">
              </div>
        
              <div  style="height: 80px; color:#fff;">
                  <div class="templateDisplay" v-if="item.templateId==0">Empty Project: <br> This project is an empty project.</div>
                  <div class="templateDisplay" v-if="item.templateId==1">Vue:<br>This project includes basic package of Vue.</div>
                  <div class="templateDisplay" v-if="item.templateId==2">React: <br>This project contains basic components of react.</div>
                  <div class="templateDisplay" v-if="item.templateId==3">Angular:<br>This project contains basic components of Angular.</div>
                  <div class="templateDisplay" v-if="item.templateId==4">static: <br>This project includes just a static html page.</div>
                  <div class="templateDisplay" v-if="item.templateId==5">Vanilla:<br>Create a Vanilla project to code. </div>
               
              </div>
          
              </li>
            <!-- <li class="rencentused">
              <div style="height: 120px;">
                <img src="../../assets/img/vuetemplate.png" width="200px" height="130px">
              </div>
              <div  style="height: 80px; color:#fff;">
                  <div style="margin-top: 25px;margin-left: 10px;">Vue</div>
                  <div style="padding-top: 10px;padding-left: 10px;"> <icon-svg name="yanjing" style="padding-top: 3px;" ></icon-svg>    701.1k codebox</div>
              </div>
            </li>
            <li class="rencentused">
              <div style="height: 120px;">
                <img src="../../assets/img/reacttemplate.png" width="200px" height="130px">
              </div>
              <div  style="height: 80px; color:#fff;">
                  <div style="margin-top: 25px;margin-left: 10px;">React</div>
                  <div style="padding-top: 10px;padding-left: 10px;"> <icon-svg name="yanjing" style="padding-top: 3px;" ></icon-svg>    531.1k codebox</div>
              </div>
            </li>
            -->
             
          </ul>
        </div>
      </el-card>
    </div>
  </template>
  
  <script>
    export default {
      data () {
        return {
          ideProjectTemplates: []
        }
      },
      methods: {
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
        }
      },
      activated () {
        this.getIdeProjectTemplates()
      }
    }
  </script>
  
  <style scoped>
    .templateDisplay{
      margin-top: 12px;margin-left: 10px;
    }
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
  
  