<template>
    <div v-bind:class="{ 'black-bg': modalOpen === true }">
      <h1>공지사항 게시판</h1>
      <button id="writeButton" class="btnClass" v-on:click="getPopUpModal"> 글쓰기</button>
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
            <td class="noticeDetail" v-on:click="fnDetail(row.noticeNo)"> {{ row.noticeSj }}</td>
            <td>{{ row.registerId }}</td>
            <td>{{ row.registDt}}</td>
          </tr>
          </tbody>
        </table>
    </div>
    <div class="black-bg" v-if="modalOpen === true">
      <div class="white-bg">
          <form id="writeForm">
            <div class="btnCloseModal">
              <button id="btnCloseModal" v-on:click="closePopUpModal">X</button>
            </div>
            <div id="noticeNo" >
              번호 : {{noticeNo}}
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
              <button type="button" class="btnClass" v-on:click="fnDelete(noticeNo)">삭제</button>
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
      noticeSj:'',
      noticeCn:'',
      noticeNo:'',
      deleteArr:[]
    }
  },
  mounted() {
    this.load();
  },
  methods: {
    load() {
      axios.get('/vueApi/selectNoticeList/').then(res => { 
        this.boardList = res.data;
      });
    },
    getPopUpModal(){
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
      if(this.noticeNo == undefined || this.noticeNo == ''){
        url = "/vueApi/insertNoticeForm/";
      }else{
        url = "/vueApi/updatetNoticeForm/";
      }

      axios.post(url, params).then(res => { 
        if(res.status == 200){
          this.noticeNo= '';
          alert("저장이 완료되었습니다.");
          this.modalOpen = false;
        }else{
          alert("저장을 실패했습니다. 잠시후 다시시도해 주세요.");
        } 
      });  
    },
    fnDelete(idx){
      /**삭제 배열에 추가*/
      this.deleteArr.push(idx);
      let params =  {
        "deleteArr": this.deleteArr,
      };
      console.log(params.deleteArr);
      axios.post("/vueApi/deleteNoticeForm/", params).then(res => { 
        if(res.status == 200){
           alert("삭제가 완료되었습니다..");
           this.modalOpen = false;
        }else{
          console.log(res);
           alert("삭제 실패되었습니다..");
        }  
      });
    },
    closePopUpModal(){
      this.modalOpen = false;
    },
    fnDetail(idx){
      this.modalOpen = true;
      
      let params =  {
        "noticeNo": idx,
      };

     axios.post("/vueApi/selectNoticeDetail/", params).then(res => { 
        if(res.status == 200){
         this.noticeSj = res.data.detail.noticeSj;
         this.noticeCn = res.data.detail.noticeCn;
         this.noticeNo = res.data.detail.noticeNo;
        }  
      });
   
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
.black-bg {
  display: flex;
  align-items: center;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.432);
  position: fixed;
  padding: 20px;
}
.white-bg {
  width: 50%;
  background-color: white;
  padding: 20px;
  border-radius: 8px;
}

.modal-exit-btn {
  margin-top: 30px;
}

.modal-exit-btn:hover {
  cursor: pointer;
}
.chkBoxClass{
  background:rgba(0,0,0,0.8);
}
#modalPopUp{
 
}
#noticeNo{
  font-size:18px;
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
.noticeDetail{
  text-decoration : underline;
  cursor:pointer;
}
</style>
