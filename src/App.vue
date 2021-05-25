<template>
  <div id="app" @click="openBalloon2">
    <div class="contact" >
        <div class="title">
            タスク設定
        </div>
        <div name="form1">
            <ul>
                <li>
                    <label>タスク名称：</label>
                    <select v-model="inputTaskName" name="taskname">
                    <option value="">作業名を入力</option>
                    <option value="0W**-**412-00【ボディー1】">0W**-**412-00【ボディー1】</option>
                    <option value="YA**14702【ﾎﾞﾃﾞｨｰ2】">YA**14702【ﾎﾞﾃﾞｨｰ2】</option>
                    <option value="123**-**8R-D000">123**-**8R-D000</option>
                    <option value="www.google.com">0W**-**421-10【アスクルドライブ】</option>
                    </select>
                </li>
                <li>
                    <label>ID:</label>
                    <input type="text" v-model="inputTaskId" placeholder="ID" />
                </li>
                <li>
                    <label>次工程：</label>
                    <select v-model="inputTaskNextP" name="taskname">
                    <option value="">次工程を入力</option>
                    <option value="63V-1">63V-1</option>
                    <option value="63V-2">63V-2</option>
                    <option value="63V-3">63V-3</option>
                    </select>
                <li>
                    <label>詳細:</label>
                    <input type="text" v-model="inputTaskDetail" placeholder="詳細" />
                </li>
                <li>
                    <label>納期:</label>
                    <input type="text" v-model="inputTaskNoki" placeholder="納期" />
                </li>
                <li>
                    <label>処理:</label>
                    <input type="text" v-model="inputTaskSyori" placeholder="処理" />
                </li>
            </ul>
            <button class="btn" @click="addTask" >タスク追加</button>
            <button class="btn" @click="deleteTask">タスク取消</button>
        </div>
    </div>
    <div style="height: 1px;border: 1px solid black;">
    </div>
    <div style="top:480px;height: 1200px; width: 2400px; border: 5px solid black; position: absolute;margin: 0 auto">
      <vue-draggable-resizable
        v-for="(item, index) in tasks" :key="index+item.name"
        :x=item.position.x
        :y=item.position.y
        :w="200"
        :h="150"
        :parent="true"
        :debug="false"
        :min-width="100"
        :min-height="100"
        :isConflictCheck="true"
        :snap="true"
        :snapTolerance="1"
        @dragging="onDrag"
        @resizing="onResize"
        @refLineParams="getRefLineParams"
        class="task">
        <div @mousedown="mousedown(index)">
        <div class="title">{{item.id}}</div>
        <div>
        {{item.name}}<button class="btn-bg-img" @click="openBalloon"></button><br>
        開始：{{item.startTime}},終了：{{item.endTime}}<br>
        次工程：{{item.nextP}}
        </div>
        <div>
          <p class="input-balloon" v-show="isFocused" @click="closeBalloon">詳細：{{item.detail}}<br>納期：{{item.noki}}<br>処理：{{item.syori}}<br></p>
          <div class="backdrop" v-show="isFocused" @click="closeBalloon"></div>
        </div>
        </div>
      </vue-draggable-resizable>
      <!--辅助线-->
      <span class="ref-line v-line"
            v-for="item in vLine"
            :key="item.id"
            v-show="item.display"
            :style="{ left: item.position, top: item.origin, height: item.lineLength}"
      />
      <span class="ref-line h-line"
            v-for="item in hLine"
            :key="item.id"
            v-show="item.display"
            :style="{ top: item.position, left: item.origin, width: item.lineLength}"
      />
      <!--辅助线END-->
         <div>
         <p class="input-balloon2" v-show="isFocused2" @click="closeBalloon2">
          品番/品名<br>工程/時間<br>次工程/生産数<br>【コメント】<br>受注記録管理No，<br>納期<br>
         受注数<br>特殊工程：引渡日<br>実績加工機<br>変更加工機<br>
          </p>
         <div class="backdrop" v-show="isFocused2" @click="closeBalloon2"></div>
         </div>
    </div>
<div class ="div-a">
<center>
<table width="90%" border="1">
  <tbody >
    <tr>
      <td align="center"><br>
      <center>
      <table style="BORDER-RIGHT: #d8d8d8 1px solid; BORDER-TOP: #d8d8d8 1px solid; BORDER-LEFT: #d8d8d8 1px solid; BORDER-BOTTOM: #d8d8d8 1px solid" cellspacing="0" cellpadding="10" width="2200" align="center">
        <tbody>
          <tr>
            <td valign="bottom" bgcolor="#5a6d45" colspan="2" height="5"> <font face="Verdana" color="#f2eac0" size="+1"><b>理想の新生産計画</b></font></td>
          </tr>
          <tr>
            <td bgcolor="#c6c785" colspan="2" height="5"></td>
          </tr>
          <tr valign="top">
            <td style="PADDING-RIGHT: 20px; PADDING-LEFT: 20px; PADDING-BOTTOM: 20px; PADDING-TOP: 20px" align="middle">
            <h3><font face="Verdana" color="#c62633">スケジュール</font></h3>
            <table cellpadding="0" width="97%" >
              <tbody>
                <tr>
                  <td width="70"><font size="+1"><b>2021年</b></font></td>
                  <td align="right"></td>
                  <td align="middle" width="100"><font size="+1"><b>6月/1日</b></font></td>
                  <td></td>
                  <td width="70"></td>
                </tr>
              </tbody>
            </table>
            <table style="BORDER-COLLAPSE: collapse" bordercolor="#aaaaaa" cellspacing="0" cellpadding="3" width="100%" bgcolor="#ffffff" border="1">
              <tbody >
                <tr>
                  <th align="middle" width="120"><font color="#000000"><b> </b></font></th>
                  <th align="middle" width="50" colspan="4"><font color="#000000">9:00</font></th>
                  <th align="middle" width="50" colspan="4"><font color="#000000">10:00</font></th>
                  <th align="middle" width="50" colspan="4"><font color="#000000">11:00</font></th>
                  <th align="middle" width="50" colspan="4"><font color="#000000">12:00</font></th>
                  <th align="middle" width="50" colspan="4"><font color="#000000">13:00</font></th>
                  <th align="middle" width="50" colspan="4"><font color="#000000">14:00</font></th>
                  <th align="middle" width="50" colspan="4"><font color="#000000">15:00</font></th>
                  <th align="middle" width="50" colspan="4"><font color="#000000">16:00</font></th>
                  <th align="middle" width="50" colspan="4"><font color="#000000">17:00</font></th>
                  <th align="middle" width="50" colspan="4"><font color="#000000">18:00</font></th>
                </tr>
                <tr>
                  <td align="left" ><font color="#000000"><b>②63V-1号機</b></font>
                  <button class="btn-bg-img" ></button>
                  </td>
                  <td align="middle"  class="td_l" height="150"><font color="#000000"></font></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                </tr>
                <tr>
                  <td align="left" ><font color="#000000"><b>②63V-2号機</b></font>
                  <button class="btn-bg-img" ></button>
                  </td>
                  <td align="middle"  class="td_l" height="150"><font color="#000000"></font></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                </tr>
                <tr>
                  <td align="left" ><font color="#000000"><b>②63V-3号機</b></font>
                  <button class="btn-bg-img" ></button>
                  </td>
                  <td align="middle"  class="td_l" height="150"><font color="#000000"></font></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                  <td align="middle" class="td_l"></td>
                  <td align="middle" class="td_m1"></td>
                  <td align="middle" class="td_m2"></td>
                  <td align="middle" class="td_r"></td>
                </tr>
              </tbody>
            </table>
            </td>
          </tr>
          <tr>
            <td bgcolor="#c6c785" colspan="2" height="10"></td>
          </tr>
          <tr>
            <td align="middle" bgcolor="#5a6d45" colspan="2"><font color="#f2eac0" size="2">Copyright
(C) All Rights Reserved.</font></td>
          </tr>
        </tbody>
      </table>
      </center>
      <br>
      </td>
    </tr>
  </tbody>
</table>
</center>
</div>
  </div>

</template>

<script>
import VueDraggableResizable from './components/vue-draggable-resizable'
import './components/vue-draggable-resizable.css'
export default {
  name: 'app',
  components: {
    VueDraggableResizable
  },
  data () {
    return {
      vLine: [],
      hLine: [],
      isFocused: false,
      isFocused2: false,
      inputIndex: -1,
      inputTaskId: '',
      inputTaskName: '',
      inputTaskNextP: '',
      inputTaskDetail: '',
      inputTaskNoki: '',
      inputTaskSyori: '',
      inputStart: '',
      inputEnd: '',
      inputKi: '',
      newTask: {
        id: 0,
        name: '',
        nextP: '',
        detail: '',
        noki: '',
        syori: '',
        ki: '',
        startTime: '',
        endTime: '',
        position: {
          x: 0,
          y: 0,
          w: 100,
          h: 100
        }
      },
      tasks: [
      ]
    }
  },
  methods: {
    // 辅助线回调事件
    getRefLineParams (params) {
      const { vLine, hLine } = params
      let id = 0
      this.vLine = vLine.map(item => {
        item['id'] = ++id
        return item
      })
      this.hLine = hLine.map(item => {
        item['id'] = ++id
        return item
      })
    },
    onDrag: function (x, y) {
      var temp = 9 + Math.floor(x / 200)
      this.tasks[this.inputIndex].startTime = temp
      this.tasks[this.inputIndex].endTime = temp + 1
      this.x = x
      this.y = y
      console.log(this.inputIndex + ',' + this.tasks.length)
      if (this.inputIndex === 2 && this.tasks.length === 4) {
        this.tasks[3].position.x = x + 200
      }
    },
    onResize: function (x, y, width, height) {
      this.x = x
      this.y = y
      this.width = width
      this.height = height
      var temp = 9 + Math.floor((x + width) / 200)
      this.tasks[this.inputIndex].endTime = temp
    },
    // task追加
    addTask () {
      this.newTask.id = this.inputTaskId
      this.newTask.name = this.inputTaskName
      this.newTask.nextP = this.inputTaskNextP
      console.log('this.inputTaskNextP=' + this.inputTaskNextP)
      this.newTask.detail = this.inputTaskDetail
      this.newTask.noki = this.inputTaskNoki
      this.newTask.syori = this.inputTaskSyori
      this.newTask.position.x = 0
      this.newTask.position.y = 0
      var tempTask = JSON.parse(JSON.stringify(this.newTask))
      this.tasks.push(tempTask)
      this.inputIndex++
    },
    // task削除
    deleteTask () {
      this.tasks.pop()
      this.inputIndex--
    },
    openBalloon () {
      this.isFocused = true
    },
    closeBalloon () {
      this.isFocused = false
    },
    openBalloon2 (e) {
      console.log(e.pageX + ',' + e.pageY)
      if (e.pageX < 140 || e.pageX > 180) {
        return
      }
      if (e.pageY < 670 || e.pageY > 1010) {
        return
      }
      if (this.isFocused2 === true) {
        this.isFocused2 = false
      } else {
        this.isFocused2 = true
      }
      console.log('open2')
    },
    closeBalloon2 () {
      this.isFocused2 = false
    },
    mousedown (index) {
      console.log('mousedown:' + index)
      this.inputIndex = index
    }
  }
}
</script>
<style>

  .title {
        height:30px;
        background-color: rgba(100, 100, 100, .1);
  }
  .setting {
     width:300px;
     height:300px;
     border:1px solid black;
     background-color: rgb(255, 255, 255);
  }
  .task {
     background-color: rgb(255, 255, 255);
  }
  .input-balloon {
  position: relative;
  padding: 20px;
  background: #ff0;
  z-index: 9;
}
.input-balloon::before{
  content: '';
  position: absolute;
  left: 20px;
  top: -15px;
  display: block;
  width: 0;
  height: 0;
  border-right: 15px solid transparent;
  border-bottom: 15px solid #ff0;
  border-left: 15px solid transparent;
  z-index: 9;
}
.input-balloon2 {
  position: absolute;
  padding: 20px;
  background: #ff0;
  z-index: 9;
  width: 300px;
}
.input-balloon2::before{
  content: '';
  position: absolute;
  left: 20px;
  top: -15px;
  display: block;
  width: 0;
  height: 0;
  border-right: 15px solid transparent;
  border-bottom: 15px solid #ff0;
  border-left: 15px solid transparent;
  z-index: 9;
}
.backdrop {
  opacity: 0;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 1;
}
.div-relative{position:relative; color:#000; border:1px solid #000; width:500px; height:400px}
.div-a{ position:absolute; left:30px; top:580px; width:200px; height:100px; z-index: -1;border: 5PX}
/* 背景 */
.div-b{ position:absolute; left:50px; top:10px; background:#FF0; width:400px; height:200px}
/* 背景为黄色 */
.div-c{ position:absolute; left:80px; top:80px; background:#00F; width:300px; height:300px}
/* DIV背景颜色为蓝色 */
.table{
   BORDER-COLLAPSE: collapse;
   border-width: 1px;
   border-color: #aaaaaa;
   cellspacing: 0;
   cellpadding: 3;
   width: 100%;
   bgcolor: #ffffff;
   border: 1;
}/*设置表格最外层边框实线*/
.th{
    color: white;
    background-color: rgba(38, 45, 59, 0.767);
}
.td_l{
    border-left: 1px;
    border-right: 0px;
}
.td_m1{
    border-left: 1px dashed;
    border-right: 1px dashed;
}
.td_m2{
    border-left: 0px dashed;
    border-right: 1px dashed;
}
.td_r{
    border-left: 0px;
    border-right: 1px solid;
    bordercolor: "#aaaaaa"
}
.tr{
    vertical-align: top;
    border: 1px dashed;
}/*设置单元格边框横向虚线*/
.btn-bg-img {
  width: 20px;
  height: 20px;
  background-image: url('../static/images/detail.png');
}
.body{
            font:14px/28px "微软雅黑";
        }
        .contact *:focus{outline :none;}
        .contact{
            float: left;
            width: 560px;
            height: auto;
            background: #f6f6f6;
            margin: 20px auto;
            padding: 10px;
        }
        .contact ul {
            width: 500px;
            margin: 0 auto;
        }
        .contact ul li{
            border-bottom: 1px solid #dfdfdf;
            list-style: none;
            padding: 12px;
        }
        .contact ul li label {
            width: 120px;
            display: inline-block;
            float: left;
        }
        .contact ul li input[type=text],.contact ul li input[type=password]{
            width: 220px;
            height: 25px;
            border :1px solid #aaa;
            padding: 3px 8px;
            border-radius: 5px;
        }
        .contact ul li input:focus{
            border-color: #000;
        }
        .contact ul li input[type=text]{
            transition: padding .25s;
            -o-transition: padding  .25s;
            -moz-transition: padding  .25s;
            -webkit-transition: padding  .25s;
        }
         .contact ul li select{
            width: 240px;
            height: 25px;
            border :1px solid #aaa;
            padding: 3px 8px;
            border-radius: 5px;
        }
         .contact ul li select{
            transition: padding .25s;
            -o-transition: padding  .25s;
            -moz-transition: padding  .25s;
            -webkit-transition: padding  .25s;
        }
        .contact ul li input[type=password]{
            transition: padding  .25s;
            -o-transition: padding  .25s;
            -moz-transition: padding  .25s;
            -webkit-transition: padding  .25s;
        }
        .btn{
            position: relative;
            left: 200px;
            width: 100px;
            height: 40px;
        }
        .tips{
            color: rgba(0, 0, 0, 0.5);
            padding-left: 10px;
        }
        .tips_true,.tips_false{
            padding-left: 10px;
        }
        .tips_true{
            color: green;
        }
        .tips_false{
            color: red;
        }
</style>
