<template>
  <div>
  <h1 class="question px-4 py-2">Questioner</h1>
  <div v-if="Isquestion">
   <div>それって{{question}}？</div>
   <button @click="answerYes()" class="btn border shadow-sm pointer mx-3 my-2 d-block hovergray" type="submit">はい</button>
   <button @click="answerNo()" class="btn border shadow-sm pointer mx-3 my-2 d-block hovergray" type="submit">いいえ</button>
   <button @click="answerLike()" class="btn border shadow-sm pointer mx-3 my-2 d-block hovergray" type="submit">どちらでもない</button>
  </div>
  <div v-else>
    それは、{{answer}}ですね！
  </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data(){
    return{
      question:'',
      questionData:"",
      LangCodeArr:[],
      NoLangCode:[],
      YesLangCode:[],
      Ids:[],
      CategoryLevel:0,
      Isquestion:true,
      answer:""
    }
  },
  mounted(){
    
    axios.get("http://haseapp.weblike.jp/api/questioner/A00009?CATEGORY_LEVEL=0")
    .then(res => {
      this.question = res.data[0]['LANG_NAME'];
      this.questionData = res.data[0];
      this.LangCodeArr.push(res.data[0]); 
      })
  },
  methods:{
    answerYes(){
      this.YesLangCode.push(this.questionData.LANG_CODE)
      this.connection()
    },
    answerNo(){
      this.NoLangCode.push(this.questionData.LANG_CODE)
      this.connection()
    },
    answerLike(){
      this.Ids.push(this.questionData.ID)
      this.connection()
    },
    connection(){
      var yes = this.YesLangCode.length > 0 ? "&LANG_CODES=" + this.YesLangCode + "&LANG_CODES_2=" + this.YesLangCode :"";
      var no =  this.NoLangCode.length > 0 ? "&NO_LANG_CODES="+ this.NoLangCode:"";
      var like =this.Ids.length > 0 ?"&IDS=" + this.Ids:"";
      axios.get("http://haseapp.weblike.jp/api/questioner/A00009?CATEGORY_LEVEL="
      +this.CategoryLevel+yes+no+like)
      .then(res => {
        if(res.data[0] == null){
        this.Isquestion = false;
        }else{
        this.answer = res.data[0]["CATEGORY_NAME"];
        this.question = res.data[0]['LANG_NAME'];
        this.questionData = res.data[0];
        this.LangCodeArr.push(res.data[0]); 
        }
      })
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
