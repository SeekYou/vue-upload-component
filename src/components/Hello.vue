<template>
  <div class="hello">
    <div class="imgArea">
      <ul>
        <li v-for="(item,index) in mmm" v-bind:key="item.id">
          <img v-if="item.data!=''" v-bind:src="item.data||index"></img>
          <input type="file" title=" " v-bind:id="item.name" v-on:change="pushImg($event,index)"></input>
          <span v-if="item.data!=''" v-on:click="delImg($event,index)"></span>
        </li>
      </ul>
    </div>
    <div class="tips" v-if="flag.change">添加成功</div>
  </div>
</template>

<script>
export default {
  name: 'hello',
  data () {
    return {
      flag:{
        change:false
      },
      mmm:[{
        name:"AA",
        data:''
      },{
        name:"BB",
        data:''
      },{
        name:"CC",
        data:''
      },{
        name:"DD",
        data:''
      },{
        name:"EE",
        data:''
      }]
    }
  },
  methods:{
    pushImg:function(e,i){
      let mm=this.mmm,flag=this.flag,file=e.target,reader=new FileReader();
      console.log(file.files[0]);
      let realFile=file.files[0];
      let fileType=realFile.type.split('/')[0];
      if (fileType!=='image') {
        alert('请选择图片上传');
        this.mmm[i].data='';
        var dom=document.getElementById(this.mmm[i].name);
      console.log(dom);
      dom.value='';
        return false;
      }
      reader.readAsDataURL(file.files[0]);
      var getPromise=new Promise(function(resolve,reject){
        reader.onload=function(){
          resolve(reader);
        };
        reader.onerror=function(){
          reject(reader);
        }
      })
      getPromise.then(function(data){
        mm[i].data=data.result;
        flag.change=true;
        setTimeout(()=>{
          flag.change=false;
        },1000)
      }).catch(error=>console.log(error))
      // mm[i].data=this.result;
      //   flag.change=true;
      //   setTimeout(()=>{
      //     flag.change=false;
      //   },1000)
    },
    delImg:function(e,i){
      this.mmm[i].data='';
      var dom=document.getElementById(this.mmm[i].name);
      console.log(dom);
      dom.value='';
      
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  position: relative;
  display: inline-block;
  width: 80px;
  height: 80px;
  margin: 0 10px;
  border: 1px solid #000;
}
li img{
  position: relative;
  z-index: 2;
  width: 100%;
    height: 100%;
}
li span{
  position: absolute;
  width: 20px;
  height: 20px;
  top: -10px;
  right: -10px;
  z-index: 3;
  background: url(../assets/closeimg.png) no-repeat scroll 0 0 transparent; 
  background-size: cover;
}
a {
  color: #42b983;
}
input{
  position: absolute;
  display: inline-block;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  /*opacity: 0;*/
  /*border: 1px solid #eee;*/
  /*background: rgba(0,0,0,0);*/
  /*background-color: transparent;*/
  background: url(../assets/addimg.jpg) no-repeat;
  background-size: 100% 100%;
  z-index: 1;
}
input::-webkit-file-upload-button,button {
  display: none;
}

.tips{
  position: absolute;
    top: 30%;
    left: 0;
    bottom: 0;
    right: 0;
    margin: auto;
    background: #ddd;
  display: inline-block;
    width: 100px;
    height: 100px;
    border: 1px solid #aaa;
    text-align: center;
    line-height: 100px;
}
</style>
