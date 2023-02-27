<template>
  <div class="demo1">
    <el-row>
      <el-col :span="8">
        <div id="blockly-div"></div>
      </el-col>
      <el-col :span="8">
        <div id="code-box">
          <code v-html="code">
          </code>
        </div>
      </el-col>
      <el-col :span="8">
        <div id="one-piece">
          <div id="one-piece-part">
          </div>
        </div>
      </el-col>
      <el-button @click="exec" type="primary" class="exec-btn">执行</el-button>
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
        // kind，分类，categoryToolbox 为分类工具箱
        "kind": "categoryToolbox",
        // contents，内容
        "contents": [
          {
            "kind": "category",
            "name": "人物",
            "colour": '#bb2241',
            "contents": [
              // 工具箱引入
              {
                "kind": "block",
                "type": "Luffy",
              },
              {
                "kind": "block",
                "type": "Zoro",
              }
            ]
          },
          {
            "kind": "category",
            "name": "地点",
            "colour": '#9fff58',
            "contents": [
              // 工具箱引入
              {
                "kind": "block",
                "type": "Location"
              }
            ]
          },
          {
            "kind": "category",
            "name": "动作",
            "colour": '#de991a',
            "contents": [
              // 工具箱引入
              {
                "kind": "block",
                "type": "Speak"
              }
            ]
          }
        ]
      },
      // Blockly 生成的代码
      code: null,
      // 块定义
      blocksJsonArray: [
        {
          "type": "Luffy",
          "message0": "人物：%1",
          "args0": [
            {
              "type": "field_image",
              "src": "static/luffy.jpeg",
              "width": 16,
              "height": 16,
              "alt": "*"
            }
          ],
          "colour": "#bb2241",
          "nextStatement": null,
          "tooltip": "Luffy tips"
        },
        {
          "type": "Zoro",
          "message0": "人物：%1",
          "args0": [
            {
              "type": "field_image",
              "src": "static/zoro.jpeg",
              "width": 16,
              "height": 16,
              "alt": "*"
            }
          ],
          "colour": "#bb2241",
          "nextStatement": null,
          "tooltip": "Zoro tips"
        },
        {
          "type": "Location",
          "message0": "地点：%1",
          "args0": [
            {
              "type": "field_dropdown",
              "name": "location",
              "options": [
                ["司法岛", "EniesLobby.png"],
                ["水之都", "WaterSeven.jpeg"]
              ]
            },
          ],
          "colour": "#9fff58",
          "nextStatement": null,
          "previousStatement": null,
          "tooltip": "Location tips"
        },
        {
          "type": "Speak",
          "message0": "说话：%1",
          "args0": [
            {
              "type": "field_input",
              "name": "content",
              "text": "",
            },
          ],
          "colour": "#de991a",
          "nextStatement": null,
          "previousStatement": null,
          "tooltip": "Speak tips"
        }
      ]
    }
  },
  methods: {
    // 初始化 Blockly
    initBlockly() {
      // 块定义
      Blockly.defineBlocksWithJsonArray(this.blocksJsonArray);
      // 添加生成器函数
      this.blockGenerator();
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
    },
    // 添加生成器函数，即每个自定义块要生成什么代码
    blockGenerator() {
      Blockly.JavaScript['Luffy'] = function(block) {
        return `this.character('luffy');<br>`;
      };
      Blockly.JavaScript['Zoro'] = function(block) {
        return `this.character('zoro');<br>`;
      };
      Blockly.JavaScript['Location'] = function(block) {
        let argument0 = block.getFieldValue('location');
        return `this.location('${argument0}');<br>`;
      };
      Blockly.JavaScript['Speak'] = function(block) {
        let argument0 = block.getFieldValue('content');
        return `this.speak('${argument0}');<br>`;
      };
    },
    // 人物
    character(name) {
      let img = document.createElement("img"); 
      img.src = `static/${name}.jpeg`; 
      img.className = 'character';
      let part = document.getElementById('one-piece-part'); 
      part.appendChild(img);
    },
    // 地点
    location(name) {
      let onePiece = document.getElementById('one-piece'); 
      onePiece.style.backgroundImage = `url(static/${name})`;
    },
    // 说话
    speak(content) {
      let span = document.createElement("span"); 
      span.innerText = content;
      span.className = 'word';
      let part = document.getElementById('one-piece-part'); 
      part.appendChild(span);
    },
    exec() {
      let handleCode = this.code.replaceAll('<br>', '');
      eval(handleCode);
    }
  },
  mounted() {
    this.initBlockly();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  .demo1 {
    position: relative;
  }

  #blockly-div {
    width: 80%;
    height: 400px;
    margin-left: 80px;
  }

  #code-box {
    width: 80%;
    height: 378px;
    padding: 10px;
    border: 1px solid #c6c6c6;
  }

  #one-piece {
    position: relative;
    width: 80%;
    height: 400px;
    border: 1px solid #c6c6c6;
    background-repeat: no-repeat;
    background-size: auto 100%;
    background-position: center;
  }

  .character {
    height: 80px;
  }

  .word {
    position: absolute;
    left: 100px;
    width: 120px;
    height: 80px;
    background: #fff;
    padding: 5px;
    border-radius: 10px;
  }

  .word:before {
    content:"";
    position: absolute;
    width: 0;
    height: 0;
    border-top: 13px solid transparent;
    border-right: 26px solid #fff;
    border-bottom: 13px solid transparent;
    margin: 13px 0 0 -25px;
  }

  #one-piece-part {
    position: absolute;
    bottom: 50px;
    left: 50px;
  }

  .exec-btn {
    position: absolute;
    bottom: 0;
    right: 577px;
  }
</style>
