<template>
  <div>
  <h1 class="question px-4 py-2">Questioner</h1>
    <div>それって{{question}}？</div>
   <button @click="answerYes(1)" class="btn border shadow-sm pointer mx-3 my-2 d-block hovergray" type="submit">はい</button>
   <button @click="answerNo(0)" class="btn border shadow-sm pointer mx-3 my-2 d-block hovergray" type="submit">いいえ</button>
   <button @click="answerLike(2)" class="btn border shadow-sm pointer mx-3 my-2 d-block hovergray" type="submit">どちらでもない</button>

  </div>
</template>

<script>
import axios from 'axios';
export default {
  data(){
    return{
      question:'',
      LangCodeArr:[],
      CategoryLevel:0
    }
  },
  mounted(){
    axios.get("http://127.0.0.1/api/questioner/A00000?CATEGORY_LEVEL=0")
    .then(res => {
      this.question = res.data[0]['LANG_NAME'];
      this.LangCodeArr.push(res.data[0]['LANG_CODE']); 
      })
  },
  methods:{
    answerYes(ans){
      axios.get("http://127.0.0.1/api/questioner/A00001?LANG_CODES="+this.LangCodeArr.join(",")+"&LANG_CODES_2="+this.LangCodeArr.join(",")+"&CATEGORY_LEVEL="+this.CategoryLevel)
      .then(res => {
        this.question = res.data[0]['LANG_NAME'];
        this.LangCodeArr.push(res.data[0]['LANG_CODE']); 
      })
      
    },
    answerNo(ans){
    },
    answerLike(ans){

    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
._title{
    color:#e74c3c;
}
.pointer{
    cursor: pointer;
}

.question{
    color:#ecf0f1;
    background:#e74c3c;
}

.hovergray:hover{
    background:rgba(250,250,250,1);
}
</style>
