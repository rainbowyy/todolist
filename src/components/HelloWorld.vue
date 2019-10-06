<template>
  <div class="hello">
    <div class="top">
      <p>任务列表</p>
    </div>
    <div class="center">
      <p class="title">添加任务</p>
      <el-input v-model="input" placeholder="请输入内容" @change="addList()"></el-input>
      <div class="show">
        <el-row>
          <el-col :md="10">
            <span style="text-align:left;">总共有：{{num}}{{text}}</span>
          </el-col>
          <el-col :md="14">
            <el-button @click="allList">所有任务</el-button>
            <el-button type="primary" @click="unsuccessList">未完成任务</el-button>
            <el-button type="success" @click="successList">已完成任务</el-button>
          </el-col>
        </el-row>
      </div>
      <div class="list">
        <p class="title">任务列表：</p>
        <div v-show="!listLength" style="padding:10px 0; border-bottom:1px solid #efefef;">
          <p>暂无任务列表</p>
        </div>
        <ul>
          <li class="list1" v-for="(item,index) in dataList" :key="index" @dblclick="editList(index)">
            <el-row v-show="item.flag">
              <el-col :md="16" class="left">
                <el-checkbox class="left check" v-model="item.isChecked">{{item.title}}</el-checkbox>
              </el-col>
              <el-col :md="8">
                <el-button
                  type="danger"
                  icon="el-icon-close"
                  circle
                  size="mini"
                  @click="deleteList(index)"
                ></el-button>
              </el-col>
            </el-row>
            <el-input v-show="!item.flag" v-model="editext" @change="editText(index)"></el-input>
          </li>
         
             
    

        
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
//存储到本地
var store={
  save(key,value){ //存
  localStorage.setItem(key,JSON.stringify(value)) //value值是字符串

  },
  fetch(key){//取
 return JSON.parse(localStorage.getItem(key)) || []
  }
}

var dataList=store.fetch('new-list')

export default {
  name: "HelloWorld",
  data() {
    return {
      input: "",
      dataList: dataList,
      num:'0',
      text:'个任务',
      editext:''
    };
  },
  watch:{  //watch擅长处理的场景：一个数据影响多个数据

    // dataList:function(){ //监听dataList是否发生变化，不能明确监听到其中每一项的属性是否发生了变化
    //   store.save('new-list',this.dataList) 
    // }
  
    dataList:{
      handler(){
       store.save('new-list',this.dataList)  
      //  console.log(this.dataList)
       this.num=this.dataList.length
       console.log(this.num)
      },
      deep:true
    }
  },
mounted(){
this.num=this.dataList.length
},
  methods: {
    addList() {
      this.dataList.push({
        title: this.input,
        isChecked: false,
        flag:true
      });
    },
    deleteList(id) {
      console.log(id);
      this.dataList.splice(id, 1);
    },
    allList(){ //所有任务列表
    this.num=this.dataList.length
    this.text="个任务"
    },
    unsuccessList(){  //未完成任务列表
    let sum=this.dataList.filter((item)=>{
    return !item.isChecked
    })
    this.num=sum.length
    this.text='个未完成任务'
    },
    successList(){ //已完成任务列表
   let sum=this.dataList.filter((item)=>{
    return item.isChecked
    })
    this.num=sum.length
    this.text="个已完成任务"
    },
    editList(id){ //双击编辑任务列表
    this.dataList[id].flag=false
    this.editext=this.dataList[id].title
    },
    editText(id){ //编辑完成
    console.log(id)
    this.dataList[id].title=this.editext
    this.dataList[id].flag=true


    }
  },
  computed:{ //computed擅长处理的场景：一个数据受多个数据影响
listLength(){
  return this.dataList.length
}
 

  }
};
</script>
<style scoped>
.hello {
  margin: 0;
  padding: 0;
}
.top {
  background: red;
  color: #fff;
  line-height: 64px;
}
.center {
  margin: 0 auto;
  width: 60%;
}
.title {
  text-align: left;
}
.show {
  margin: 10px 0;
}
.show span {
  text-align: left;
}
ul,
li {
  list-style: none;
}
.list1 {
  border-bottom: 1px solid #eee;
  padding: 10px 0;
}
.left {
  text-align: left;
}

</style>
