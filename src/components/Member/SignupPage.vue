<template>

<div class="bawbody">
  <div class="mt-4">
    <router-link to="/"><legend style="margin-top : 30px">회원가입</legend></router-link>
  </div>
  <form @submit.prevent="submitForm" class="mpcontainer h-100" autocomplete="off">
    <div class="form-group row">
      <label for="nickname" class="form-label mt-4">닉네임</label>
      <input type="text" class="form-control" placeholder="닉네임을 입력해주세요" id="nickname" v-model="nickname" required>
      <small v-if="condition[0] == true">이미 사용 중인 닉네임입니다 :(</small>
      <small v-if="condition2[0] == true">닉네임을 입력해주세요 :(</small>
    </div>

    <div class="form-group row">
      <label for="memberId" class="form-label mt-4">아이디</label>
      <input type="text" class="form-control" placeholder="아이디를 입력해주세요" id="memberId" v-model="memberId" required>
      <small v-if="condition[1] == true">이미 사용 중인 아이디입니다 :(</small>
      <small v-if="condition2[1] == true">아이디를 입력해주세요 :(</small>
    </div>

    <div class="form-group row">
      <label for="pw" class="form-label mt-4">비밀번호</label>
      <input type="password" class="form-control" placeholder="비밀번호를 입력해주세요" id="pw" v-model="pw" required>
      <small v-if="condition2[2] == true">비밀번호를 입력해주세요 :(</small>
    </div>

    <div class="form-group row">
      <label for="birthYear" class="form-label mt-4">태어난 연도</label>
      <input type="text" class="form-control" placeholder="태어난 연도를 입력해주세요 (숫자 4자리)" id="birthYear" v-model="birthYear" required>
      <small v-if="condition2[5] == true">태어난 연도를 입력해주세요 :(</small>
    </div>

    <div class="form-group row">
      <input type="hidden" class="form-control" id="tier" value="0">
    </div>

    <div class="form-group row">
      <label for="pwQuestion" class="form-label mt-4">비밀번호 찾기 질문</label>
        <select class="form-select" id="pwQuestion" v-model="pwQuestion" required>
          <option value="" disabled selected>질문을 선택해주세요</option>
          <option>태어난 도시는 어디인가요?</option>
          <option>가장 기억에 남는 장소는 어디인가요?</option>
          <option>가장 감명 깊게 읽은 책은 무엇인가요?</option>
          <option>가장 행복했던 순간은 언제인가요?</option>
          <option>현재 소속은 어디인가요?</option>
        </select>
      <small v-if="condition2[3] == true">질문을 선택해주세요 :(</small>
    </div>

    <div class="form-group row">
      <label for="pwAnswer" class="form-label mt-4">비밀번호 찾기 답변 </label>
      <input type="text" class="form-control" placeholder="질문에 대한 답을 입력해주세요" id="pwAnswer" v-model="pwAnswer" required>
      <small v-if="condition2[4] == true">답을 입력해주세요 :(</small>
    </div>

    <div class="form-group row">
      <label for="gender" class="form-label mt-4">성별</label>
        <select class="form-select" id="gender" v-model="gender" required>
          <option value="" disabled selected>성별을 선택해주세요</option>
          <option>남</option>
          <option>여</option>
        </select>
        <small v-if="condition2[6] == true">성별을 선택해주세요 :(</small>
    </div>

    <div class="form-group row">
      <label for="email" class="form-label mt-4">이메일</label>
      <input type="text" class="form-control" id="email" placeholder="example@naver.com" v-model="email" required  @submit="checkEmail">
      <small v-if="condition[2] == true">이미 사용 중인 이메일입니다 :(</small>
      <small v-if="condition2[7] == true">올바른 양식을 입력해주세요 :(</small>
    </div>
    
    <div class="form-group row">
      <label for="region" class="form-label mt-4">지역</label>
      <select class="form-select" id="region" v-model="region" required>
        <option value="" disabled selected>지역을 선택해주세요</option>
        <option value='강원'>강원</option>
        <option value='경기'>경기</option>
        <option value='경남'>경남</option>
        <option value='경북'>경북</option>
        <option value='광주'>광주</option>
        <option value='대구'>대구</option>
        <option value='대전'>대전</option>
        <option value='부산'>부산</option>
        <option value='서울'>서울</option>
        <option value='울산'>울산</option>
        <option value='인천'>인천</option>
        <option value='전남'>전남</option>
        <option value='전북'>전북</option>
        <option value='제주'>제주</option>
        <option value='충남'>충남</option>
        <option value='충북'>충북</option>
      </select>
      <small v-if="condition2[8] == true">지역을 선택해주세요 :(</small>
    </div>
    
    <div class="d-grid gap-2 mt-5">
      <button class="btn btn-lg btn-primary" type="submit" style="margin-bottom : 80px">가입하기</button>
    </div>
  </form>
</div>

</template>

<script>
import axios from 'axios';
axios.defaults.baseURL="http://ec2-13-125-131-254.ap-northeast-2.compute.amazonaws.com";

export default {
  name: "SignupPage",
  data() {
    return {
      condition : [false,false,false],
      condition2 : [false,false,false,false,false,false,false,false,false],
      memberId : "",
      pw : "",
      pwQuestion : "",
      pwAnswer : "",
      nickname : "",
      birthYear : "",
      gender : "",
      email : "",
      tier : "",
      region : "",
    }
  },

  watch : {
    nickname(a) {

      if (a == "") {
        this.condition2[0] = true;
      } else {
        this.condition2[0] = false;
      }  

      axios.post('/members/check/nickname',{
        nickname : a,
      })
      .then(res => {
        if (res.data == false) {
          this.condition[0] = true;
        } else {
          this.condition[0] = false;
        }
      })
      .catch(error => console.log(error));
    },

    memberId(a) {

      if (a == "") {
        this.condition2[1] = true;
      } else {
        this.condition2[1] = false;
      }  

      axios.post('/members/check/memberid', {
        memberId : a,
      })
      .then(res => {
        if (res.data == false) {
          this.condition[1] = true;
        } else {
          this.condition[1] = false;
        } 
      })
      .catch(error => console.log(error));
    },
    
    email(a) {

      let regEmail = /^([\w-]+(?:\.[\w-]+)*)@((?:[\w-]+\.)*\w[\w-]{0,66})\.([a-z]{2,6}(?:\.[a-z]{2})?)$/i;
      
      if (a == "" || regEmail.test(a) == false) {
        this.condition2[7] = true;
      } else {
        this.condition2[7] = false;
      }

      axios.post('/members/check/email', {
        email : a,
      })
      .then(res => {
        if (res.data == false) {
          this.condition[2] = true;
        } else {
          this.condition[2] = false;
        }
      })
      .catch(error => console.log(error));
    },

    pw(a) {

      if (a == "") {
        this.condition2[2] = true;
      } else {
        this.condition2[2] = false;
      }
    },

    pwQuestion(a) {

      if (a == "") {
        this.condition2[3] = true;
      } else {
        this.condition2[3] = false;
      }
    },

    pwAnswer(a) {

      if (a == "") {
        this.condition2[4] = true;
      } else {
        this.condition2[4] = false;
      }
    },

    birthYear(a) {

      if (a == "") {
        this.condition2[5] = true;
      } else {
        this.condition2[5] = false;
      }
    },

    gender(a) {

      if (a == "") {
        this.condition2[6] = true;
      } else {
        this.condition2[6] = false;
      }
    },

    region(a) {

      if (a == "") {
        this.condition2[8] = true;
      } else {
        this.condition2[8] = false;
      }
    }
  },

  methods : {
    submitForm() {

      if(this.condition[0] || this.condition2[0]) {
        return false;
      }

      if(this.condition[1] || this.condition2[1]) {
        return false;
      }

      if(this.condition[2] || this.condition2[7]) {
        return false;
      }

      if(this.condition2[2]) {
        return false;
      }

      if(this.condition2[3]) {
        return false;
      }

      if(this.condition2[4]) {
        return false;
      }

      if(this.condition2[5]) {
        return false;
      }

      if(this.condition2[6]) {
        return false;
      }
      
      if(this.condition2[8]) {
        return false;
      }
      
      axios.post("/member",{
        data : 
          {
            memberId : this.memberId,
            pw : this.pw,
            pwQuestion : this.pwQuestion,
            pwAnswer : this.pwAnswer,
            nickname : this.nickname,
            birthYear : this.birthYear,
            email : this.email,
            gender : this.gender,
            region : this.region,
            tier : this.tier
          },
        "success": true,
        "code": 0,
        "message": "성공"
      })
      .then(res => {
        console.log(res.data);
        axios.post("/members/tier/" + res.data.data)
        .then(res => {
          console.log(res.data.data);
        })
        alert("회원가입이 완료되었습니다.");
        this.$router.push('/');
      })
      .catch(err => {
        console.log(err);
      });
    },

    // checkNickname() {
    //   axios.post('/members/check/nickname', {
    //       nickname : this.nickname,
    //   })
    //   .then(res => {
    //     if(res === null) {
    //       console.log("null")
    //     }
    //     console.log("notnull");
    //     if (res == false) {
    //       this.condition[0] = true;
    //     } else {
    //       this.condition[0] = false;
    //     }
    //   })
    //   .catch(error => console.log(error));
    // },

    // checkMemberId() {
    //   axios.post('/members/check/memberid', {
    //       memberId : this.memberId,
    //   })
    //   .then(res => {
    //     if(res === null) {
    //       console.log("null")
    //     }
    //     console.log("notnull");
    //     if (res == false) {
    //       this.condition[1] = true;
    //     } else {
    //       this.condition[1] = false;
    //     } 
    //   })
    //   .catch(error => console.log(error));
    // },

    // checkEmail() {
    //   axios.post('/members/check/email', {
    //       email : this.email,
    //   })
    //   .then(res => {
    //     if(res === null) {
    //       console.log("null")
    //     }
    //     console.log("notnull");
    //     if (res == false) {
    //       this.condition[2] = true;
    //     } else {
    //       this.condition[2] = false;
    //     }
    //   })
    //   .catch(error => console.log(error));
    // }

  },
}
</script>

<style scoped>

.mpdiv {
  margin-bottom: 10px;
}

.mpcontainer {
    width: 500px;
}

legend {
  text-align: center;
  font-size: 40px;
}

label {
  padding-top: 10px;
}

small {
  color : red;
}

select option[value=""][disabled] {
	display: none;
}

</style>