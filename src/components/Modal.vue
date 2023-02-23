<template>
  <div id="modalPopUp">
      <form>
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
</template>
<script>
 export default {
    name: 'modalPopUp',
    components: {}
  }
</script>