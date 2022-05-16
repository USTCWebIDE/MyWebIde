/* eslint-disable no-unused-vars */
/* eslint-disable no-unused-vars */
<template>
    <div>
      <template v-for="menu in this.menuData">
        <el-submenu :key="menu.fileColumn.id+''" :index="menu.fileColumn.id+''"  v-if="menu.fileColumn.fileType=='package'"
        >
            <template slot="title"  >
              <div   @mouseover="overFile(menu.fileColumn)" @mouseout="outFile" style="display: flex;justify-content: start;">
                  <span  @click="dataClick(menu.fileColumn)"><icon-svg :name="menu.fileColumn.suffix" class="site-sidebar__menu-icon" ></icon-svg></span>
                  <span style="width: 100px;"@click="dataClick(menu.fileColumn)" slot="title">{{menu.fileColumn.fileName}}</span>
                  <span >
                    <!-- <ul class="filetoolsul" v-if="overFileId==menu.fileColumn.id"> -->
                      <ul class="filetoolsul"  > 
                      <li class="filetools" @click="clickNewFile(menu)">
                     
                        <el-tooltip class="item" effect="dark" content="new file" placement="top">
                          <icon-svg name="newfile" class="site-sidebar__menu-icon" ></icon-svg>
                        </el-tooltip>
                      </li>
                      <li class="filetools" @click="clickNewFileFolder(menu)">
                        <el-tooltip class="item" effect="dark" content="new filefolder" placement="top">
                          <icon-svg name="newfilefolder" class="site-sidebar__menu-icon" ></icon-svg>
                        </el-tooltip>
                      </li>
                      <li class="filetools"  @click="clickRenameFileFolder(menu)">
                        <el-tooltip class="item" effect="dark" content="rename" placement="top">
                          <icon-svg name="filerename" class="site-sidebar__menu-icon" ></icon-svg>
                        </el-tooltip>
                      </li>
                      <li class="filetools" @click="clickDeleteFileFolder(menu)">
                        <el-tooltip class="item" effect="dark" content="delete" placement="top">
                          <icon-svg name="deletefile" class="site-sidebar__menu-icon" ></icon-svg>
                        </el-tooltip>
                      </li>
                      
                    </ul>
                  </span>
              </div>
            </template>
            <menu-tree :menuData="menu.children" ></menu-tree>
        </el-submenu>
        <el-menu-item  :key="menu.fileColumn.id+''" :index="menu.fileColumn.id+''"  v-if="menu.fileColumn.fileType=='file'"
        >
            <template slot="title"  >
              <div  @mouseover="overFile(menu.fileColumn)" @mouseout="outFile" style="display: flex;justify-content: start;" >
                  <span  v-if="menu.fileColumn.fileName!=null" @click="dataClick(menu.fileColumn)"> 
                    <icon-svg :name="menu.fileColumn.suffix" class="site-sidebar__menu-icon" >
                    </icon-svg>
                  </span>
                  <span  v-if="menu.fileColumn.fileName!=null" style="width: 100px;" slot="title" @click="dataClick(menu.fileColumn)">{{menu.fileColumn.fileName}}</span>
                  <span >
                    <ul class="filetoolsul" >
                      <li class="filetools" @click="clickRenameFileFolder(menu)">
                        <el-tooltip class="item" effect="dark" content="rename" placement="top">
                        <icon-svg name="filerename" class="site-sidebar__menu-icon" ></icon-svg>
                        </el-tooltip>
                      </li>
                      <li class="filetools" @click="clickDeleteFileFolder(menu)">
                        <el-tooltip class="item" effect="dark" content="delete" placement="top">
                        <icon-svg name="deletefile" class="site-sidebar__menu-icon" ></icon-svg>
                        </el-tooltip>
                      </li>
                    </ul>
                  </span>
              </div>
           
            </template>
            <menu-tree :menuData="menu.children" ></menu-tree>
        </el-menu-item>
        
      </template>
      <el-dialog title="新建文件" :visible.sync="addFileVisable"
      :modal="false"
      width="30%"
      >
      <el-form :model="newFile" label-position="left" label-width="80px"  
      @keyup.enter.native="submitNewFile()">
        <el-form-item label="文件名" >
          <el-input v-model="newFile.fileColumn.fileName" placeholder="请输入文件名"  >

          </el-input></el-form-item>
      </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="addFileVisable = false" >取 消</el-button>
          <el-button @click="submitNewFile">确 定</el-button>
        </div>
      </el-dialog>

      <el-dialog title="新建文件夹" :visible.sync="addFileFolderVisable"
      :modal="false"
      
      width="30%"
      >
      <el-form :model="newFileFolder" label-position="left" label-width="80px" 
      
      @keyup.enter.native="submitNewFileFolder()">
        <el-form-item label="文件夹名" >
          <el-input v-model="newFileFolder.fileColumn.fileName" placeholder="请输入文件夹名"  >

          </el-input></el-form-item>
      </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="addFileFolderVisable = false" >取 消</el-button>
          <el-button @click="submitNewFileFolder">确 定</el-button>
        </div>
      </el-dialog>
      <el-dialog title="修改名字" :visible.sync="eiditFileNameVisible"
      :modal="false"
      width="30%"
      >
      <el-form :model="newFileFolder" label-position="left" label-width="80px" 
      
      @keyup.enter.native="submitEiditFileName()">
        <el-form-item label="新名字" >
          <el-input v-model="eiditFile.fileName"   >

          </el-input></el-form-item>
      </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="cancelRename" >取 消</el-button>
          <el-button @click="submitEiditFileName">确 定</el-button>
        </div>
      </el-dialog>
      <el-dialog title="删除警告" :visible.sync="deleteFileVisible"
      :modal="false"
      width="30%"
      @keyup.enter.native="submitDeleteFileFolder()"
      >
      <div style="text-align: center;">确定要删除文件夹及其所有文件吗？</div>
        <div slot="footer" class="dialog-footer">
          <el-button @click="deleteFileVisible=false" >取 消</el-button>
          <el-button @click="submitDeleteFileFolder">确 定</el-button>
        </div>
      </el-dialog>
    </div>
  </template>
   
  <script>
  import MenuTree from '@/views/common/MenuTree'
  export default {
    components: {
      MenuTree: MenuTree
    },
    props: ['menuData'],
    name: 'MenuTree',
    data () {
      return {
        property: 'value',
        overFileId: '',
        createFilePid: '',
        eiditFile: '',
        deleteFile: '',
        addFileVisable: false,
        addFileFolderVisable: false,
        eiditFileNameVisible: false,
        deleteFileVisible: false,
        currentPackage: '',
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
        newFileFolder: {
          children: [],
          fileColumn: {
            id: '',
            projectId: '',
            pid: '',
            suffix: '',
            fileAddr: '',
            filetext: '',
            fileName: '',
            fileType: 'package'
          }
        }
      }
  },
    methods: {
      clickRenameFileFolder (data) {
        this.eiditFile = data.fileColumn
        this.eiditFileNameVisible = true
      },
      clickDeleteFileFolder (data) {
        this.deleteFileVisible = true
        this.deleteFile = data.fileColumn
      },
      submitDeleteFileFolder () {
        this.$http({
          url: this.$http.adornUrl('/ideproject/filetable/deleteFile'),
          method: 'post',
          data: this.deleteFile
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.$message.success('delete success!')
            this.deleteFileVisible = false
            window.sessionStorage.setItem('isNeedRefreshFileContent', 1)
          } else {
            this.$message.error(data.msg)
          }
        })
      },
      cancelRename () {
        this.eiditFileNameVisible = false
        window.sessionStorage.setItem('isNeedRefreshFileContent', 1)
      },
      submitEiditFileName () {
        this.$http({
          url: this.$http.adornUrl('/ideproject/filetable/eiditFileName'),
          method: 'post',
          data: this.eiditFile
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.$message.success('rename success!')
            this.eiditFileNameVisible = false
            window.sessionStorage.setItem('isNeedRefreshFileContent', 1)
          } else {
            this.$message.error(data.msg)
          }
        })
      },
      clickNewFile (data) {
        console.log("@@@@@@@@@@")
        this.addFileVisable = true
        this.createFilePid = data.fileColumn.id
        this.newFile.fileColumn.projectId = data.fileColumn.projectId
        this.currentPackage = data.children
      },
      clickNewFileFolder (data) {
        this.addFileFolderVisable = true
       // this.createFilePid = data.fileColumn.id
        this.newFileFolder.fileColumn.projectId = data.fileColumn.projectId
        this.newFileFolder.fileColumn.pid = data.fileColumn.id
      },
      submitNewFileFolder () {
        if (this.newFileFolder.fileColumn.fileName == '') return this.$message.error('请输入文件名！')
        this.$http({
          url: this.$http.adornUrl('/ideproject/filetable/addNewFileFolder'),
          method: 'post',
          data: this.newFileFolder
        }).then(({data}) => {
          if (data && data.code === 0) {
            window.sessionStorage.setItem('isNeedRefreshFileContent', 1)
            this.addFileFolderVisable = false
            this.newFileFolder = {
              children: [],
              fileColumn: {
                id: '',
                projectId: '',
                pid: '',
                suffix: '',
                fileAddr: '',
                filetext: '',
                fileName: '',
                fileType: 'package'
              }
            }
          } else {
            this.$message.error(data.msg)
          }
        })
      },
      submitNewFile () {
        if (this.newFile.fileColumn.fileName == '') return this.$message.error('请输入文件名！')
    //    console.log('验证通过！')
        this.newFile.fileColumn.pid = this.createFilePid
        this.$http({
          url: this.$http.adornUrl('/ideproject/filetable/addNewFile'),
          method: 'post',
          data: this.newFile
        }).then(({data}) => {
          if (data && data.code === 0) {
            window.sessionStorage.setItem('isNeedRefreshFileContent', 1)
            console.log(this.menuData)
            this.addFileVisable = false
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
          } else {
            this.$message.error(data.msg)
          }
        })
      },
      overFile (data) {
        console.log('onmouseFileId:' + data.id)
        this.overFileId = data.id
      },
      outFile () {
        this.overFileId = ''
      },
      dataClick (e) { // 传递点击事件到父组件
        if (e.fileType == 'file') {
          window.sessionStorage.setItem('currentEiditFileId', e.id)
          window.sessionStorage.setItem('currentOpenFile', JSON.stringify(e))
          window.sessionStorage.setItem('currentOpenFileSuffix',e.suffix)
          window.sessionStorage.setItem('currentCodeFileText', e.filetext)
        }
      }
    }
  }
  </script>
  <style scoped>
    .filetoolsul{
      display: flex;justify-content: space-around;list-style:none;
    }
    .filetools{
      padding: 10px;
      line-height: 30px;
      opacity: 0.5;
      color: black;
    }
    .filetools:hover{
      opacity: 1;
    }
  </style>