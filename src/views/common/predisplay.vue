
<template>
    <div class="mod-home">
      <h3>Display Container:</h3>
      <el-card class="maincard">
        <div>
          <iframe name="myiframe" id="myrame" src="http://localhost:4040/#/" width="100%" height="525px">
            
        </iframe>
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
    
      },
      activated () {
    
      }
    }
  </script>
  
  <style scoped>
    .templateDisplay{
     
    }
    .maincard{
      height: 525px;
    }
    .mod-home {
      line-height: 1.5;
    }
  
  </style>
  
  