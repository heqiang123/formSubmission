<template>
  <div class="indexBox">
   <div class="collegeLeft">
      <h4>欢迎加入酷学院</h4>
     <div class="collegeLeftBox">
      <p>个人登记表</p>
      <div class="nameBox" v-for="(item,index) in testFieldArr" :key="'input'+index" @click="changeInputVal(index)">
          <p>{{item.title}}</p>
          <p>{{item.describe}}</p>
          <el-input v-model="item.nameModel" placeholder="请输入内容"></el-input>
      </div>
     </div>
     <div class="educationBox" v-for="(item,index) in selectArr" :key="'select'+index" @click='changeSelectVal(index)'>
        <p>{{item.title}}</p>
        <p>{{item.describe}}</p>
        <el-radio v-model="item.radio" :label="itemLabel.label?itemLabel.label:itemLabel.flag" v-for="(itemLabel,indexLabel) in item.labelArr" :key="indexLabel">{{itemLabel.label}}</el-radio>
     </div>
     <div class="cityBox" v-for="(item,index) in selectListArr" :key="index" @click="changeSelectListVal(index)">
       <p>{{item.title}}</p>
       <el-select v-model="item.val" placeholder="请选择">
          <el-option
            v-for="(itemOP,indexOp) in item.options"
            :key="indexOp"
            :label="itemOP.label"
            :value="itemOP.label">
          </el-option>
        </el-select>
     </div>
     <div class="tableBox" v-for="(item,index) in tableDataArr" :key="'table'+index" @click="changeTableVal(index)">
       <p>{{item.title}}</p>
       <el-table
        :data="item.tableData"
        style="width: 100%">
          <el-table-column
            prop="evaluate1"
            width="180">
          </el-table-column>
          <el-table-column
            prop="evaluate2"
            ref="tableData"
            :label=item.label1
            align="center"
            width="180">
            <template slot-scope="scope">
              <el-radio v-model="scope.row.radio" :label="scope.row.evaluate2">4</el-radio>
            </template>
          </el-table-column>
          <el-table-column
            prop="evaluate3"
            align="center"
            ref="tableData1"
            :label=item.label2>
            <template slot-scope="scope">
              <el-radio v-model="scope.row.radio" :label="scope.row.evaluate3">3</el-radio>
            </template>
          </el-table-column>
          <el-table-column
            prop="evaluate4"
            align="center"
            ref="tableData2"
            :label=item.label3>
            <template slot-scope="scope">
              <el-radio v-model="scope.row.radio" :label="scope.row.evaluate4">2</el-radio>
            </template>
          </el-table-column>
          <el-table-column
            prop="evaluate5"
            align="center"
            ref="tableData3"
            :label=item.label4>
            <template slot-scope="scope">
              <el-radio v-model="scope.row.radio" :label="scope.row.evaluate5">1</el-radio>
            </template>
          </el-table-column>
      </el-table>
     </div>
     <textarea v-model="allData" name="" id="" cols="30" rows="10"></textarea>
      <el-row>
          <el-button type="primary" @click="sunmit">提交</el-button>
      </el-row>
   </div>
   <div class="collegeRight">
    <p>添加字段</p>
    <el-row>
      <el-button plain @click="testField">单行文本</el-button>
      <el-button plain @click="tableFun">矩形单选</el-button><br>
      <el-button plain @click="select">单项选择</el-button>
      <el-button plain @click="selectList">下拉框</el-button>
    </el-row>
    <p>编辑字段</p>
    <div class="editBox">
      <p>标题</p>
      <el-input v-model="editModel" placeholder="请输入内容"></el-input>
    </div>
    <div class="editBox" v-if="inputState || selectState">
      <p>标题描述</p>
      <el-input v-model="titModel" placeholder="请输入内容"></el-input>
    </div>
    <div class="editBox" v-if="selectState">
      <p>选项</p>
      <el-input v-model="item.label" placeholder="请输入内容" v-for="(item,index) in selectArr[this.titleIndex].labelArr" :key="index"></el-input>
    </div>
    <div class="editBox" v-if="selectListState">
      <p>下拉选项</p>
      <el-input v-model="item.label" placeholder="请输入内容" v-for="(item,index) in selectListArr[this.titleIndex].options" :key="index"></el-input>
    </div>
    <div class="editBox" v-if="taableState">
      <p>表格选项</p>
      <div v-for="(item,index) in tableDataArr[this.titleIndex].tableData" :key="'tableData'+index">
        <el-input v-model="item.evaluate1" placeholder="请输入内容"></el-input>
      </div>
      <el-input v-model="titleLable1" placeholder="请输入内容"></el-input>
      <el-input v-model="titleLable2" placeholder="请输入内容"></el-input>
      <el-input v-model="titleLable3" placeholder="请输入内容"></el-input>
      <el-input v-model="titleLable4" placeholder="请输入内容"></el-input>
    </div>
   </div>

  </div>
</template>

<script>
export default {
  name: 'index',
  data () {
    return {
      allData: '',
      nameModel: '',
      tableDataLength: -1,
      index: 0,
      selectIndex: 4,
      tableIndex: -1,
      titleIndex: -1,
      describeIndex: -1,
      selectState: false,
      inputState: false,
      selectListState: false,
      taableState: false,
      tableData1: [],
      titleLable1: '',
      titleLable2: '',
      titleLable3: '',
      titleLable4: '',
      selectListArr: [
        {
          title: '你来自哪里',
          val: '',
          options: [
            {
              value: '选项1',
              label: '黄金糕'
            }, {
              value: '选项2',
              label: '双皮奶'
            }, {
              value: '选项3',
              label: '蚵仔煎'
            }, {
              value: '选项4',
              label: '龙须面'
            }, {
              value: '选项5',
              label: '北京烤鸭'
            }
          ]
        }
      ],
      selectArr: [{
        title: '留下你的大名吧',
        describe: '请告诉我们你的大名或绰号都行',
        labelArr: [
          {
            label: '选项1',
            flag: '0'
          },
          {
            label: '选项2',
            flag: '1'
          },
          {
            label: '选项3',
            flag: '2'
          },
          {
            label: '选项4',
            flag: '3'
          }
        ]
      }],
      selectArrLength: -1,
      testFieldArr: [{
        title: '留下你的大名吧',
        describe: '请告诉我们你的大名或绰号都行',
        nameModel: ''
      }],
      radio: '',
      poorRadio: '',
      evaluate: '',
      options: [{
        value: '选项1',
        label: '黄金糕'
      }, {
        value: '选项2',
        label: '双皮奶'
      }, {
        value: '选项3',
        label: '蚵仔煎'
      }, {
        value: '选项4',
        label: '龙须面'
      }, {
        value: '选项5',
        label: '北京烤鸭'
      }],
      selectCity: '',
      tableDataArr: [
        {
          title: '自我评估测试',
          label1: '较差了',
          label2: '一般了',
          label3: '还不错',
          label4: '这是我的强项',
          tableData: [{
            radio: '',
            evaluate1: `沟通技巧`,
            evaluate2: '沟通技巧:较差了',
            evaluate3: '沟通技巧:一般了',
            evaluate4: '沟通技巧:还不错',
            evaluate5: '沟通技巧:这是我的强项'
          }, {
            radio: '',
            evaluate1: '时间观念',
            evaluate2: '时间观念:较差了',
            evaluate3: '时间观念:一般了',
            evaluate4: '时间观念:还不错',
            evaluate5: '时间观念:这是我的强项'
          }, {
            radio: '',
            evaluate1: '技能熟练',
            evaluate2: '技能熟练:较差了',
            evaluate3: '技能熟练:一般了',
            evaluate4: '技能熟练:还不错',
            evaluate5: '技能熟练:这是我的强项'
          }, {
            radio: '',
            evaluate1: '2016-05-03',
            evaluate2: '2016-05-03:较差了',
            evaluate3: '2016-05-03:一般了',
            evaluate4: '2016-05-03:还不错',
            evaluate5: '2016-05-03:这是我的强项'
          }]
        }
      ],
      editModel: '',
      titModel: ''
    }
  },
  watch: {
    // 监听实时修改input标题框
    editModel (val) {
      if (this.inputState) {
        this.testFieldArr[this.titleIndex].title = val
      } else if (this.selectState) {
        this.selectArr[this.titleIndex].title = val
      } else if (this.selectListState) {
        this.selectListArr[this.titleIndex].title = val
      } else {
        this.tableDataArr[this.titleIndex].title = val
      }
    },
    // 监听实时修改input描述框
    titModel (val) {
      if (this.inputState) {
        this.testFieldArr[this.titleIndex].describe = val
      } else if (this.selectState) {
        this.selectArr[this.titleIndex].describe = val
      }
    },
    // 以下是监听实时修改表头字段
    titleLable1 (val) {
      this.tableDataArr[this.titleIndex].label1 = val
    },
    titleLable2 (val) {
      this.tableDataArr[this.titleIndex].label2 = val
    },
    titleLable3 (val) {
      this.tableDataArr[this.titleIndex].label3 = val
    },
    titleLable4 (val) {
      this.tableDataArr[this.titleIndex].label4 = val
    }
  },
  methods: {
    // 点击表单修改单行文本字段
    changeInputVal (index) {
      this.inputState = true
      this.selectState = false
      this.taableState = false
      this.selectListState = false
      this.editModel = this.testFieldArr[index].title
      this.titModel = this.testFieldArr[index].describe
      this.titleIndex = index
    },
    // 点击表单修改单选框字段
    changeSelectVal (index) {
      this.selectState = true
      this.inputState = false
      this.taableState = false
      this.selectListState = false
      this.editModel = this.selectArr[index].title
      this.titModel = this.selectArr[index].describe
      this.titleIndex = index
    },
    // 点击表单修改下拉框字段
    changeSelectListVal (index) {
      this.selectListState = true
      this.inputState = false
      this.selectState = false
      this.taableState = false
      this.editModel = this.selectListArr[index].title
      this.titleIndex = index
    },
    // 点击表单修改表格字段
    changeTableVal (index) {
      this.taableState = true
      this.selectListState = false
      this.inputState = false
      this.selectState = false
      this.editModel = this.tableDataArr[index].title
      this.titleLable1 = this.tableDataArr[index].label1
      this.titleLable2 = this.tableDataArr[index].label2
      this.titleLable3 = this.tableDataArr[index].label3
      this.titleLable4 = this.tableDataArr[index].label4
      this.tableDataArr[index].tableData[0].evaluate2 = `${this.tableDataArr[index].tableData[0].evaluate1}:${this.titleLable1}`
      this.tableDataArr[index].tableData[0].evaluate3 = `${this.tableDataArr[index].tableData[0].evaluate1}:${this.titleLable2}`
      this.tableDataArr[index].tableData[0].evaluate4 = `${this.tableDataArr[index].tableData[0].evaluate1}:${this.titleLable3}`
      this.tableDataArr[index].tableData[0].evaluate5 = `${this.tableDataArr[index].tableData[0].evaluate1}:${this.titleLable4}`
      this.titleIndex = index
    },
    // 添加单选框
    select () {
      this.selectIndex++
      this.selectArr.push({
        title: '留下你的大名吧',
        describe: '请告诉我们你的大名或绰号都行',
        labelArr: [
          {
            label: `选项${this.selectIndex}`,
            flag: this.selectIndex
          },
          {
            label: `选项${this.selectIndex + 1}`,
            flag: this.selectIndex
          },
          {
            label: `选项${this.selectIndex + 2}`,
            flag: this.selectIndex
          },
          {
            label: `选项${this.selectIndex + 3}`,
            flag: this.selectIndex
          }
        ]
      })
      this.selectIndex += 3
    },
    // 添加下拉框
    selectList () {
      this.selectListArr.push({
        title: '你来自哪里',
        val: '',
        options: [
          {
            value: '选项1',
            label: '黄金糕'
          }, {
            value: '选项2',
            label: '双皮奶'
          }, {
            value: '选项3',
            label: '蚵仔煎'
          }, {
            value: '选项4',
            label: '龙须面'
          }, {
            value: '选项5',
            label: '北京烤鸭'
          }
        ]
      })
    },
    // 单行文本
    testField () {
      this.testFieldArr.push({
        title: '留下你的大名吧',
        describe: '请告诉我们你的大名或绰号都行',
        nameModel: ''
      })
      console.log(this.testFieldArr)
    },
    // 添加表格
    tableFun () {
      this.tableIndex++
      this.tableDataArr.push({
        title: '自我评估测试',
        label1: `较差了${this.tableIndex}`,
        label2: `一般了${this.tableIndex}`,
        label3: `还不错${this.tableIndex}`,
        label4: `这是我的强项${this.tableIndex}`,
        tableData: [{
          evaluate1: `沟通技巧`,
          evaluate2: `沟通技巧:较差了`,
          evaluate3: `沟通技巧:一般了`,
          evaluate4: `沟通技巧:还不错`,
          evaluate5: `沟通技巧:这是我的强项`
        }, {
          evaluate1: `时间观念`,
          evaluate2: `时间观念:较差了`,
          evaluate3: `时间观念:一般了`,
          evaluate4: `时间观念:还不错`,
          evaluate5: `时间观念:这是我的强项`
        }, {
          evaluate1: `技能熟练`,
          evaluate2: `技能熟练:较差了`,
          evaluate3: `技能熟练:一般了`,
          evaluate4: `技能熟练:还不错`,
          evaluate5: `技能熟练:这是我的强项`
        }, {
          evaluate1: `2016-05-03`,
          evaluate2: `2016-05-03:较差了`,
          evaluate3: `2016-05-03:一般了`,
          evaluate4: `2016-05-03:还不错`,
          evaluate5: `2016-05-03:这是我的强项`
        }]
      })
    },
    // 提交
    sunmit () {
      let inputData = ''
      let inputArr = []
      let selectArr1 = []
      let selectData1 = ''
      let selectListArr1 = []
      let selectListData1 = ''
      let tableDataArr1 = []
      this.testFieldArr.forEach(res => {
        inputData = `${res.title},${res.describe},${res.nameModel}`
        inputArr.push(inputData)
      })
      this.selectArr.forEach(res => {
        selectData1 = `${res.title},${res.describe},${res.radio}`
        selectArr1.push(selectData1)
      })
      this.selectListArr.forEach(res => {
        selectListData1 = `${res.title},${res.val}`
        selectListArr1.push(selectListData1)
      })
      this.tableDataArr.forEach(res => {
        res.tableData.forEach(res1 => {
          tableDataArr1.push(res1.radio)
        })
      })
      this.allData = `单行文本${inputArr.join()}--------------单选项${selectArr1.join()}--------------下拉框${selectListArr1.join()}----------表格${tableDataArr1.join()}`
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less">
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.indexBox{
  width:100%;
  display: flex;
  justify-content: space-between;
  padding: 20px;
  box-sizing: border-box;
    .collegeLeft{
      width: 68%;
      border-radius: 5px;
      /* padding:10px; */
      .collegeLeftBox{
        padding: 10px 0 10px 40px;
        border-bottom:1px solid #eee;
      }
      h4{
        background: #4276e9;
        border-radius: 5px;
        padding:10px;
        color:#fff;
        margin:0;
      }
      p{
        padding:5px 10px;
      }
      .nameBox{
        padding:10px;
        p{
          font-size: 14px;
          margin:10px;
        }
        p:nth-chold(2){
          font-size: 12px;
        }
      }
      .educationBox{
        padding:10px 0 10px 40px;
        border-bottom:1px solid #eee;
        .el-input{
          width: 80px;
        }
        .el-input__inner{
          border:none
        }
        .labelStyle{
          .el-radio__label{
            margin-left:15px
          }
        }
        p{
          font-size: 14px;
          margin:10px;
        }
        p:nth-chold(2){
          font-size: 12px;
        }
      }
      .cityBox{
        padding:10px 0 10px 40px;
        border-bottom:1px solid #eee;
      }
      .tableBox{
        padding:10px 0 10px 40px;
        border-bottom:1px solid #eee;
        .el-radio__label{
          display:none
        }
        .el-input__inner{
          border:none
        }
        p{
          font-size: 14px;
          margin:10px;
        }
      }
    }
    .collegeRight{
      width: 30%;
      border-radius: 5px;
      // padding:10px;
      p{
        background: #4276e9;
        padding: 10px;
        color: #fff;
      border-radius: 5px;
      }
      .editBox{
        p{
          background: #fff;
          color:black;
        }
      }
    }
}

</style>
