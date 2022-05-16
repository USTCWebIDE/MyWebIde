/* eslint-disable no-unused-expressions */
/* eslint-disable eqeqeq */
/* eslint-disable no-mixed-spaces-and-tabs */
/* eslint-disable no-undef */
/* eslint-disable no-unused-vars */
/* eslint-disable no-unused-vars */
/* eslint-disable no-mixed-spaces-and-tabs */
<template>
     <div>
      <h3 style=" color:black;">Project WorkSpace</h3>
       <div class="projectworkspace" id="projectworkspace">
          <div class="dragdivleft"id="dragdivleft">
            <div class="workspace">
                <div class="filescontenthead">
                    <div style="width: 49%;line-height: 30px;text-align: left;margin-left: 10px;">Files</div>
                    <div style="width: 49%;text-align: center;margin-left: 10px;margin-top: -8px;">
                      <ul style="display: flex;justify-content: space-around;list-style: none;">
                        <li class="filetools" @click="addFatherFile">
                          <el-tooltip class="item" effect="dark" content="add new file" placement="top">
                          <icon-svg name="addfile" class="site-sidebar__menu-icon"></icon-svg>
                        </el-tooltip>
                        </li>
                        <li class="filetools" @click="addFatherFileFolder">
                          <el-tooltip class="item" effect="dark" content="add new folder" placement="top">
                          <icon-svg name="addfolder" class="site-sidebar__menu-icon"></icon-svg>
                           </el-tooltip>
                        </li> 
                          <li class="filetools" @click="refreshProject">
                            <el-tooltip class="item" effect="dark" content="refresh" placement="top">
                              <icon-svg name="refresh" class="site-sidebar__menu-icon"></icon-svg>
                          </el-tooltip>
                          </li>
                          <li class="filetools" @click="runproject">
                            <el-tooltip class="item" effect="dark" content="run this project" placement="top">
                              <icon-svg name="run" class="site-sidebar__menu-icon"></icon-svg>
                          </el-tooltip>
                          </li>
                          <li class="filetools" @click = "submitDownload">
                            <el-tooltip class="item" effect="dark" content="download the project file" placement="top">
                              <icon-svg name="download" class="site-sidebar__menu-icon"></icon-svg>
                          </el-tooltip>
                          </li>
                      </ul>
                    </div>
                </div>
                <div class="filescontent">
                          <el-menu
                            default-active="1-1-1-1"
                            background-color="#fff"
                            text-color="black"
                            active-text-color="blue">
                            <menu-tree :menuData="menuList"
                            ></menu-tree>
                          </el-menu>
                </div>
            </div>
            <div class="dragdiv" id="dragdiv" v-on:mousedown="dragstart">
            </div>
          </div>
          
          <div class="dragdivright" id="dragdivright" >
            <div class="filecolumn">
                <li class="openingfile" v-for="item in openFileList" >
                  <div style="width: 90%; " @click="changeOpenFile(item)"> 
                    {{item.fileName}}
                    <div class="openfileFlag" v-if="openfileId==item.id"> </div>
                  </div>
                  <div class="openfileclose" @click="removeOpenFile(item)" >
                    <icon-svg name="close" class="site-sidebar__menu-icon" style="width: 12px;height: 12px;">
                    </icon-svg></div>
                 
                </li>
            </div>
            <div class="filedisplay" id="filecodediv" 
            >  
            <div id="code">
                <template>
                  <codemirror   @focus="clearTimer"
                  @blur="setTimer"  v-model="currentCodeFile" :options="options" class="codemirrorstyle" ></codemirror>   
                </template>
            </div>
            </div>
          </div>
        </div>
        <el-dialog :title="'新建'+newFileType" :visible.sync="addFileVisable"
        :modal="false"
        width="30%"
        >
        <el-form :model="newFile" label-position="left" label-width="80px"  
        @keyup.enter.native="submitAddFile">
          <el-form-item :label="newFileType+'名'" >
            <el-input v-model="newFile.fileColumn.fileName" :placeholder="'请输入'+newFileType+'名'"  >
            </el-input></el-form-item>
        </el-form>
          <div slot="footer" class="dialog-footer">
            <el-button @click="addFileVisable = false" >取 消</el-button>
            <el-button @click="submitAddFile">确 定</el-button>
          </div>
        </el-dialog>
        <el-dialog title="请确认是否要下载此项目" :visible.sync="downloadVisable"
        :modal="false"
        width="30%"
        center 
        >
          <div slot="footer" class="dialog-footer">
            <el-button @click="submitDownload" type="success">确 定</el-button>
            <el-button @click="downloadVisable = false" >取 消</el-button>
          </div>
        </el-dialog>
     </div>
</template>
  
  <script>
    import MenuTree from './MenuTree'
    import {codemirror} from 'vue-codemirror'
    import 'codemirror/lib/codemirror.css'
    import 'codemirror/keymap/sublime' // sublime编辑器效果
    import 'codemirror/theme/dracula.css'// 配置里面也需要theme设置为monokai
    import 'codemirror/mode/vue/vue.js' // 配置里面也需要mode设置为vue
    import 'codemirror/addon/selection/active-line' // 光标行背景高亮，配置里面也需要styleActiveLine设置为true
    import 'codemirror/addon/hint/show-hint'
    import 'codemirror/addon/hint/html-hint'
    import 'codemirror/addon/hint/show-hint.css'
    import Vue from 'vue'
    // import 'codemirror/addon/hint/sql-hint'
    export default {
      components: {
        MenuTree,
        codemirror
      },
      data () {
        return {
          loading: '',
          newfileName: '',
          downloadVisable: false,
          newFile: {
            children: [],
            fileColumn: {
              id: '',
              projectId: '',
              pid: '',
              suffix: '',
              fileAddr: '',
              filetext: '',
              fileName: '',
              fileType: 'file'
            }
          },
          addFileVisable: false,
          newFileType: '',
          projectName: '',
          menuList: [],
          openFileList: [],
          lastwidth: 300,
          openfileId: '',
          currentCodeFile: '//Welcome to MyWebIDE!\r\n//Please open or create a file to code.',
          suffix: 'text',
          timer: '',
         // code: '', // 编辑器绑定的值
          options: {
            tabSize: 4, // 缩进格式
            theme: 'default', // 主题，对应主题库 JS 需要提前引入
            lineNumbers: true, // 显示行号
            line: true,
            mode: 'text/x-vue',
            autofocus: true,
            styleActiveLine: true, // 高亮选中行
           // focus: "this.clearTimer",
           // blur: "this.setTimer",
            extraKeys: {'Ctrl': 'autocomplete'}, // 自定义快捷键
            hintOptions: {
              completeSingle: true // 当匹配只有一项的时候是否自动补全
            }
          }
        }
      },
      destroyed () {
        clearInterval(this.timer)
      },
  created () {
       // this.getProjectContent()
        // window.sessionStorage.setItem('currentCodeFileText', '//Welcome to MyWebIDE!\r\n//Please open or create a file to code.')
        // window.sessionStorage.removeItem('currentEiditFileId')
        // window.sessionStorage.removeItem('currentOpenFile')
        // this.openFileList = []
        // window.sessionStorage.removeItem('currentOpenFileSuffix')
  },
  mounted () {
        this.timer = setInterval(this.getCurrentCodeFileText, 200)
  },
  activated () {
        window.sessionStorage.removeItem('currentEiditFileId')
        this.getProjectContent()
        this.openFileList = []
        window.sessionStorage.setItem('currentCodeFileText', '//Welcome to MyWebIDE!\r\n//Please open or create a file to code.')
    
        window.sessionStorage.removeItem('currentOpenFile')
  },
  methods: {
        runproject () {
          this.startLoading('正在启动项目...\n第一次启动项目会进行初始化工作，请稍作等待...')
          let currentEiditProjectId = window.sessionStorage.getItem('currentEiditProjectId')
          if (currentEiditProjectId != null) {
            this.$http({
              url: this.$http.adornUrl('/ideproject/projecttable/runProjectById?projectId=' + currentEiditProjectId),
              method: 'post'
            }).then(data => {
              if (data && data.code == 0) {
                console.log('启动成功！')
              }
            })
          }
         // console.log(res)
          // console.log('等待后台响应')
          
          this.$router.push({name: 'displaycontainer'})
          this.endLoading()
        },
        startLoading (msg) {
          this.loading = Vue.prototype.$loading({
            lock: true,
            text: msg,
            background: 'rgba(255,255,255,.4)',
            target: document.querySelector('.selectTemplateDiv') // 设置加载动画区域
          })
        },
        endLoading () {
          this.loading.close()
        },
        async   submitDownload () {
          let downloadProjectId = window.sessionStorage.getItem('currentEiditProjectId')
           // console.log(downloadProjectId)
          if (downloadProjectId == null) return this.$message.error('系统繁忙，请刷新页面！')
          this.startLoading('努力加载中')
    
          const resp = await this.$http({
            url: this.$http.adornUrl('/ideproject/projecttable/info?projectId=' + downloadProjectId),
            method: 'get'
          })
    
          if (resp.data.code === 0) {
            // console.log(resp)
            this.projectName = resp.data.projectTable.projectName
          }
          const res = await this.$http({
            url: this.$http.adornUrl('/ideproject/filetable/downloadProject?projectId=' + downloadProjectId),
            method: 'get',
            responseType: 'blob'
          })
          // console.log(res)
          const content = res.data
          const fileName = this.projectName + '.zip' // 文件名称
      // 如果不确定文件类型,type可以写空字符串
          const bolb = new Blob([content], { type: '' })
          if ('download' in document.createElement('a')) {
            const link = document.createElement('a')
            link.download = fileName
            link.style.display = 'none'
        // URL.createObjectURL(bolb) = blob:http://localhost:8080/a34a8a20-acf2-3f21-bc22-45994d9f0290
            link.href = URL.createObjectURL(bolb)
            document.body.appendChild(link)
            link.click()
            URL.revokeObjectURL(link.href)
            document.body.removeChild(link)
          }
          this.endLoading()
    },
        refreshProject () {
          this.loading = Vue.prototype.$loading({
            lock: true,
            text: '正在刷新目录...',
            background: 'rgba(255,255,255,.4)',
            target: document.querySelector('.selectTemplateDiv') // 设置加载动画区域
          })
          clearInterval(this.timer)
          this.openFileList = []
          window.sessionStorage.removeItem('currentEiditFileId')
          window.sessionStorage.removeItem('currentOpenFile')
          window.sessionStorage.setItem('currentCodeFileText', '//Welcome to MyWebIDE!\r\n//Please open or create a file to code.')
          this.timer = setInterval(this.getCurrentCodeFileText, 200)
          this.loading.close()
        },
        submitAddFile () {
          let currentEiditProjectId = window.sessionStorage.getItem('currentEiditProjectId')
          if (currentEiditProjectId != null) {
            this.newFile.fileColumn.projectId = currentEiditProjectId
            this.newFile.fileColumn.pid = 0
            if (this.newFile.fileColumn.fileType == 'file') {
              this.$http({
                url: this.$http.adornUrl('/ideproject/filetable/addNewFile'),
                method: 'post',
                data: this.newFile
              }).then(({ data }) => {
                if (data && data.code == 0) {
                  window.sessionStorage.setItem('isNeedRefreshFileContent', 1)
                  this.addFileVisable = false
                } else this.$message.error(data.msg)
              })
            } else if (this.newFile.fileColumn.fileType == 'package') {
              this.$http({
                url: this.$http.adornUrl('/ideproject/filetable/addNewFileFolder'),
                method: 'post',
                data: this.newFile
              }).then(({ data }) => {
                if (data && data.code == 0) {
                  window.sessionStorage.setItem('isNeedRefreshFileContent', 1)
                  this.addFileVisable = false
                } else this.$message.error('创建文件夹失败！')
              })
            }
          }

          this.newFile = {
            children: [],
            fileColumn: {
              id: '',
              projectId: '',
              pid: '',
              suffix: '',
              fileAddr: '',
              filetext: '',
              fileName: '',
              fileType: 'file'
            }
          }
        },
        addFatherFile () {
          this.newFileType = '文件'
          this.newFile.fileColumn.fileType = 'file'
          this.addFileVisable = true
        },
        addFatherFileFolder () {
          this.newFile.fileColumn.fileType = 'package'
          this.newFileType = '文件夹'
          this.addFileVisable = true
        },
        changeOpenFile (data) {
          this.openfileId = data.id
          this.clearTimer()
          window.sessionStorage.removeItem('currentOpenFile')
          window.sessionStorage.setItem('currentOpenFile', JSON.stringify(data))
          window.sessionStorage.setItem('currentOpenFileSuffix', data.suffix)
          window.sessionStorage.setItem('currentEiditFileId', data.id)
          window.sessionStorage.setItem('currentCodeFileText', data.filetext)
          this.timer = setInterval(this.getCurrentCodeFileText, 200)
        },
        removeOpenFile (data) {
          for (let i = 0; i < this.openFileList.length; ++i) {
            if (data.id == this.openFileList[i].id) {
              this.openFileList.splice(i, 1)
            }
          }
          if (this.openFileList.length == 0) {
            this.clearTimer()
            window.sessionStorage.setItem('currentOpenFileSuffix', 'text')
            window.sessionStorage.removeItem('currentEiditFileId')
            window.sessionStorage.removeItem('currentOpenFile')
            this.openFileList = []
            window.sessionStorage.setItem('currentCodeFileText', '//Welcome to MyWebIDE!\r\n//Please open or create a file to code.')
            this.openfileId = ''
            this.timer = setInterval(this.getCurrentCodeFileText, 200)
          } else {
            this.clearTimer()
            // this.currentCodeFile = this.openFileList[0].filetext
            // console.log(this.openFileList)
            window.sessionStorage.removeItem('currentOpenFile')
            window.sessionStorage.setItem('currentOpenFile', JSON.stringify(this.openFileList[0]))
            window.sessionStorage.setItem('currentEiditFileId', this.openFileList[0].id)
            window.sessionStorage.setItem('currentOpenFileSuffix', this.openFileList[0].suffix)
            this.openfileId = this.openFileList[0].id
            window.sessionStorage.setItem('currentCodeFileText', this.openFileList[0].filetext)
            this.timer = setInterval(this.getCurrentCodeFileText, 200)
          }
        },
        clearTimer () {
          clearInterval(this.timer)
        },
        setTimer () {
          let currentEiditId = window.sessionStorage.getItem('currentEiditFileId')
          window.sessionStorage.setItem('currentCodeFileText', this.currentCodeFile)
          if (currentEiditId != null) {
            for (let i = 0; i < this.openFileList.length; ++i) {
              if (this.openFileList[i].id == currentEiditId) {
                this.openFileList[i].filetext = this.currentCodeFile
                break
              }
            }
            this.$http({
              url: this.$http.adornUrl('/ideproject/filetable/saveById'),
              method: 'post',
              data: {
                'id': currentEiditId,
                'filetext': this.currentCodeFile
              }
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.getProjectContent()
              } else {
                this.$message.error('错误:部分项目文件已经不存在！')
              }
            })
          }
          this.timer = setInterval(this.getCurrentCodeFileText, 200)
        },
        getCurrentCodeFileText () {
          let suffix = window.sessionStorage.getItem('currentOpenFileSuffix')
          this.suffix = suffix
          if (suffix == 'css') { this.options.mode = 'text/css' } else if (suffix == 'vue' || suffix == 'html') { this.options.mode = 'text/x-vue' } else if (suffix == 'js') { this.options.mode = 'text/javascript' } else if (suffix == 'json') { this.options.mode = 'text/javascript' } else if (suffix == 'ts') this.options.mode = 'text/javascript'
          else this.options.mode = 'text'
          // console.log(this.suffix)
          this.currentCodeFile = window.sessionStorage.getItem('currentCodeFileText')
          let temp = window.sessionStorage.getItem('currentOpenFile')
          if (window.sessionStorage.getItem('isNeedRefreshFileContent') == 1) {
            this.getProjectContent()
            window.sessionStorage.removeItem('isNeedRefreshFileContent')
          }
          // console.log(temp)
          if (temp != null) {
            var currentOpenFile = JSON.parse(temp)
            this.openfileId = currentOpenFile.id
          } else return
          if (this.currentCodeFile != null && currentOpenFile != null) {
            for (let i = 0; i < this.openFileList.length; ++i) {
              if (currentOpenFile.id == this.openFileList[i].id) return
            }
            this.openFileList.push(currentOpenFile)
          }
        },
    
        getProjectContent () {
          let id = window.sessionStorage.getItem('currentEiditProjectId')
          if (id == null) {
            this.$http({
              url: this.$http.adornUrl('/ideproject/projecttable/getRecentProjectId?userId=' + window.sessionStorage.getItem('loginUserId')),
              method: 'get'
            }).then(({data}) => {
              if (data && data.code === 0) {
                if (data.recentProjectId != null) {
                  window.sessionStorage.setItem('currentEiditProjectId', data.recentProjectId)
                  this.$http({
                    url: this.$http.adornUrl('/ideproject/filetable/getProjectById?projectId=' + data.recentProjectId),
                    method: 'post'
                  }).then(({data}) => {
                    if (data && data.code === 0) {
                      this.menuList = data.data.children
                    } else {
                      this.$message.error('业务繁忙，请稍后！')
                    }
                  })
                }
              } else {
                this.$message.error('业务繁忙，请稍后！')
              }
            })
          } else {
            this.$http({
              url: this.$http.adornUrl('/ideproject/filetable/getProjectById?projectId=' + id),
              method: 'post'
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.menuList = data.data.children
              } else {
                this.$message.error('业务繁忙，请稍后！')
              }
            })
          }
        },
        dragstart (event) {
          let wi = 300 // 初始宽度
          let dragable = false// 默认不可拖拽
          let oldW = this.lastwidth // 记录第一次的鼠标位置

          dragable = true
          var e = event || window.event
            // oldW = e.pageX // 记录第一次的鼠标位置
                // 鼠标松开
          document.onmouseup = function () {
            if (dragable) {
              dragable = false
            };
          }
                // 鼠标指针移动
          document.onmousemove = function (event) {
            if (dragable) {
              var e = event || window.event
              var box = document.getElementById('dragdivleft')
              wi = e.pageX - oldW + 200
                  //! @#$%$#@#$%
              if (wi <= 20) { // div最低宽度
                box.style.width = '20px'
                wi = '20'
                this.lastwidth = wi
                return
              }
              if (wi >= 2000) { // div最高宽度
                box.style.width = '2000px'; wi = '2000'
                this.lastwidth = wi
                return
              }
              box.style.width = wi + 'px'
              this.lastwidth = wi
            }
          }
        }
      }
    }
  </script>
 
 
  <style scoped>
    .fileopli{
      opacity: 0.6;
    }
    .fileopli:hover{
      opacity: 1
    }
    .fileopul{
      display: flex;justify-content: space-around;list-style: none;
    }
    .filesrc{
      display: flex;justify-content: space-around;
    }
    .filefolderop{
      width: 50%;
      z-index: 999;
      opacity: 0;
    }
    .filefolderop:hover{
      opacity: 1;
    }
    .openfileclose{
      width: 5%;margin-top: -10px;
    }
    .openfileFlag{
      width:100%;
      margin-bottom: -10px;
      height: 1px;
      background-color: black;
      transition: 1s;
    }
    .openfileclose:hover{
      cursor: pointer;
    }
    .openingfileset{
      display: flex;
      justify-content: start;
       
      list-style: none;
    }
    .openingfile:hover{
      background-color: whitesmoke;
    }
    .openingfile{
      display: flex;justify-content: space-around;
      cursor: default;
      width: 124px;
      overflow: hidden;
      padding: 5px;
      padding-top: 10px;
      height: 33px;
      border: 1px solid gainsboro;
    }
    .filecolumn{
   
      width: 100%;
      height: 35px;
      border: 1px solid gainsboro;
      display: flex;justify-content: start;
      list-style: none;
 
    }
    .filedisplay{
      width: 100%;
      height: 500px;
      border: 1px solid gainsboro;
    }
    .filetools{

    }
    .filetools:hover{
      color: rgb(21, 21, 21);
      opacity: 0.6;
      cursor: pointer;
    }
    .filescontent{
      width: 100%;
      margin-top: 3px;
      height: 500px;
      overflow-y: auto;
      border: 1px solid gainsboro;
    }
    .filescontenthead:hover{
    
      background-color: whitesmoke;
    }
    .filescontenthead{
      display: flex;
      justify-content: space-around;
      width: 100%;
      height: 30px;
      overflow: hidden;
      border: 1px solid gainsboro;
    }
    .workspace{
      width: 800px;
      height: 535px;
      border: 1px solid gainsboro;
      overflow: hidden;
    }
    .projectworkspace{
      display: flex;
      width: 1400px;height: 535px;
    }
    .dragdivright{
      width:1400px;border: 1px solid gainsboro;
    }
    .dragdivleft{
      display: flex;
      justify-content: space-around;
      width: 300px;
      height: 535px;
      border: 1px solid gainsboro ;
    }
    .dragdiv{
      width:5px;
      height: 100%;
      border: 1px solid gainsboro ;
      float:right
    }
    .dragdiv:hover{
      cursor: ew-resize;
    }
  </style>
  
  