<template>
  <div class="demo1">
    <el-row>
      <el-col :span="12">
        <div id="blockly-div"></div>
      </el-col>
      <el-col :span="12">
        <div id="code-box">
          <code>
            {{code}}
          </code>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
// 引入 Blockly
import Blockly from 'blockly';  

export default {
  name: 'Demo1',
  data () {
    return {
      // Blockly 工作区实例
      workspace: null,
      // 工具箱配置
      toolbox: {
        // kind，分类，flyoutToolbox为工具箱
        "kind": "flyoutToolbox",
        // contents，内容
        "contents": [
          {
            // kind，分类，block，块
            "kind": "block",
            // type，块类型，controls_if 为官方内置的块名，表示 if 判断
            "type": "controls_if"
          },
          {
            "kind": "block",
            // type，块类型，controls_if 为官方内置的块名，表示 while 判断
            "type": "controls_whileUntil"
          }
        ]
      },
      // Blockly 生成的代码
      code: null
    }
  },
  methods: {
    // 初始化 Blockly
    initBlockly() {
      // 注入到 blockly-div 中
      this.workspace = Blockly.inject('blockly-div', {
        toolbox: this.toolbox
      });
      // 为工作区添加修改事件，触发调用 updateCode 函数
      this.workspace.addChangeListener(this.updateCode);
    },
    // 更新当前块对应的 JS 代码
    updateCode() {
      // 获取当前块对应的 JS 代码
      this.code = Blockly.JavaScript.workspaceToCode(this.workspace);
    }
  },
  mounted() {
    this.initBlockly();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #blockly-div {
    width: 800px;
    height: 400px;
    margin-left: 80px;
  }

  #code-box {
    width: 700px;
    height: 378px;
    padding: 10px;
    border: 1px solid #c6c6c6;
  }
</style>
