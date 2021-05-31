<template>
  <div class="home">
    <div class="left">
      <div class="sentence">
        <h5>句子：</h5>
        <div>
          <span v-for="(item,index) in this.textarea" :key="index">{{item}}</span>
        </div>
      </div>
      <br>
      <div class="text">
        <h5>text:</h5>
        <textarea type="text" name="" id="" style="width:50rem;height:5rem;" v-model="text"></textarea>
      </div>
      <div class="list">
        <list :clist="textarea" @itemclick="oneclick">
          <h3 slot="spantext">原因中的核心名词</h3>
        </list>
        <list :clist="textarea" @itemclick="twoclick">
          <h3 slot="spantext">原因中的谓语或状态</h3>
        </list>
        <list :clist="textarea" @itemclick="threeclick">
          <h3 slot="spantext">结果中的核心名词</h3>
        </list>
        <list :clist="textarea" @itemclick="fourclick">
          <h3 slot="spantext">结果中的谓语或状态</h3>
        </list>
      </div>
      <button @click="allsubmit()">全部提交</button>
    </div>
    <div class="right">
      <div class="login">
        <span>工号：</span>
        <input type="text" v-model="username">
        <div>
          <button @click="login()">提交</button>
        </div>
      </div>

      <div class="box">
        <div>
          <span>文件名：</span>
          <input type="text" name="" id="" v-model="filename">
        </div>
        <div>
          <span>行号：</span>
          <input type="text" name="" id="" v-model="line">
        </div>
        <div>
          <button @click="fileline()">提交</button>
        </div>
      </div>
      <h6>{{this.loginafter}}</h6>
    </div>
  </div>
</template>

<script>
import qs from 'qs';
import list from "../components/list"
export default {
  name: 'Home',
  components: {
    list,
  },
  data(){
    return {
      username:'',
      filename:'',
      line:'',
      loginafter:'',
      textarea:'',
      text:'',
      one:[],
      two:[],
      three:[],
      four:[],
      clear:false,
    }
  },
  methods:{
    login(){
    this.$axios({
      url:'http://47.97.155.31:8080/PlayTag/user/information',
      method:'post',
      headers: {
          'Content-Type': 'application/x-www-form-urlencoded'
      },
      transformRequest:[
        function(data){
            data = qs.stringify(data);
            return data;
        },
      ],
      data:{username:this.username}
    })
    .then(res=>{
      console.log(res);
      this.loginafter = res.data.data.text;
      this.username = res.data.data.username;
      console.log(this.loginafter);
    })  
    },
    fileline(){
    this.$axios({
      url:'http://47.97.155.31:8080/PlayTag/file/information',
      method:'post',
      headers: {
          'Content-Type': 'application/x-www-form-urlencoded'
      },
      transformRequest:[
        function(data){
            data = qs.stringify(data);
            return data;
        },
      ],
      data:{
        username:this.username,
        filename:this.filename,
        line:this.line,
      }
    })
    .then(res=>{
      console.log(res);
      this.textarea = res.data.data.sentence;
      this.text  = res.data.data.text;
    })      
    },
    oneclick(item){
      this.one=item;
      console.log('this.one',item);
    },
    twoclick(item){
      this.two=item;
      console.log('this.two',item);
    },
    threeclick(item){
      this.three=item;
      console.log('this.three',item);
    },
    fourclick(item){
      this.four=item;
      console.log('this.four',item);
    },
    allsubmit(){
    this.$axios({
      url:'http://47.97.155.31:8080/PlayTag/tag',
      method:'post',
      data:{
        username:this.username,
        filename:this.filename,
        line:this.line,
        re_noun:this.one,
        re_verb:this.two,
        ru_noun:this.three,
        ru_verb:this.four,
      }
    })
    .then(res=>{
      console.log(res);
      if(res.data.code==200){
        alert('成功!');
        this.one=[];
        this.two=[];
        this.three=[];
        this.four=[];
        this.textarea="";
        this.text="";
        this.line="";
        console.log(this.filename);
      }
    })        
    }
  }
}
</script>
<style scoped>
.home{
  display: flex;
}
.left{
  width: 70%;
  height: min-content;
  /* background-color: blanchedalmond; */
}
.right{
  margin-top: 5rem;
  width: 30%;
  /* height: 900px; */
  /* background-color: burlywood; */
}
.box{
  margin-top: 3rem;
}
</style>
