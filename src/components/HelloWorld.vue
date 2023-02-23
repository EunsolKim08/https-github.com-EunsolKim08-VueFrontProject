<template>
  <div class="black-bg">
      <div id="board">
        <h1>공지사항 게시판</h1>
        <button id="writeButton" class="btnClass" v-on:click="getPopUpModal"> 글쓰기</button>
        <button type="button" class="btnClass" v-on:click="fnDelete">삭제</button>
        <table id="noticeBoard">
          <thead>
          <tr>
            <th></th>
            <th>No</th>
            <th>제목</th>
            <th>작성자</th>
            <th>등록일시</th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="(row, noticeNo) in boardList" :key="noticeNo">
            <td><input type="checkbox" class="chkBoxClass"></td>
            <td>{{ row.noticeNo }}</td>
            <td> <a>{{ row.noticeSj }}</a></td>
            <td>{{ row.registerId }}</td>
            <td>{{ row.registDt}}</td>
          </tr>
          </tbody>
        </table>
      </div>
      <div id="modalPopUp" v-if="modalOpen === true">
        <form>
          <div class="btnCloseModal">
             <button id="btnCloseModal" v-on:click="closePopUpModal">X</button>
          </div>
          <div class="board-contents">
            <input id="noticeSj" type="text" v-model="noticeSj" placeholder="제목을 입력해주세요.">
          </div>
          <div class="board-contents">
            <textarea id="noticeCn" cols="30" rows="10" v-model="noticeCn"  style="resize: none;">
            </textarea>
          </div>
          <div class="common-buttons">
            <button type="button" class="btnClass" v-on:click="fnSave">저장</button>&nbsp;
            <button type="button" class="btnClass" v-on:click="fnDelete">삭제</button>
          </div>
        </form>     
    </div>
  </div>
</template>

<script>
import axios from 'axios';


export default {
  data() {
    return {
      boardList: [],
      modalOpen: false,
    }
  },
  mounted() {
    this.load();
  },
  methods: {
    load() {
      axios.get('/vueApi/selectNoticeList/').then(res => { 
        console.log(res.data);
        this.boardList = res.data;
      });
    },
    getPopUpModal(){
      console.log("글쓰기 버튼 클릭");
      this.modalOpen = true;
    },
    fnSave(){
    
      let url = "";

      let params =  {
          "noticeNo": this.noticeNo,
          "noticeSj": this.noticeSj,
          "noticeCn": this.noticeCn,
          "noticeUpendexpsrAt": "N"
      };
      if(this.noticeNo == undefined){
        url = "/vueApi/insertNoticeForm/";
      }else{
        url = "/vueApi/updatetNoticeForm/";
      }

      axios.post(url, params).then(res => { 
        if(res.status == 200){
          alert("저장이 완료되었습니다.");
          this.modalOpen = false;
        }else{
          alert("저장을 실패했습니다. 잠시후 다시시도해 주세요.");
        }    
      });
    },
    fnDelete(){
      console.log("삭제하기 버튼 클릭");
    },
    closePopUpModal(){
      this.modalOpen = false;
    },
    components: {
    
    }
  } 
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
#board{
  margin: 200px;
}
table {
       width: 100%;
       border: 1px solid #333333;
       border-collapse: collapse;
}
td, thead, th{
      padding: 10px;
      border: 1px solid #333333;
}
#writeButton{

  margin : 0px 10px 20px 75%;
}
.btnClass{
  height: 30px;
  width : 60px;
}
.black-bg{
  //background:rgba(0,0,0,0.8);
}
.chkBoxClass{
  background:rgba(0,0,0,0.8);
}
#modalPopUp{
 
}
#noticeSj{
  width: 400px;
  height: 30px;
  font-size:18px;
  margin-bottom: 10px;
}
#noticeCn{
  width: 400px;
  margin-bottom: 10px;
  font-size:18px;
}
#btnCloseModal{
  margin: 0px 0px 10px 350px;
}
</style>
