<template>
  <header>
      <div class="bs-docs-section clearfix">
        <div class="row">
          <div class="col-lg-12">
            <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
            <div class="container-fluid">
            <router-link to="/" style="text-decoration: none;"><a class="navbar-brand">Black And White</a></router-link>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarColor01" aria-controls="navbarColor01" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
            </button>

            <div class="collapse navbar-collapse" id="navbarColor01">
            <ul class="navbar-nav me-auto">
                <li class="nav-item dropdown">
                <a class="nav-link  active dropdown-toggle" data-bs-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Board</a>
                <div class="dropdown-menu">
                  <router-link to="/board" style="text-decoration: none;"><a class="dropdown-item">자유 게시판</a></router-link>
                  <!-- <router-link to="/board" style="text-decoration: none;"><a class="dropdown-item">공략 게시판</a></router-link> -->
                </div>
                </li>
                
                <li class="nav-item dropdown">
                <a class="nav-link  active dropdown-toggle" data-bs-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Game</a>
                <div class="dropdown-menu">
                  <router-link to="/chessgamepage" style="text-decoration: none;"><a class="dropdown-item">♟ Chess</a></router-link>
                  <!-- <a class="dropdown-item">🃏 Card</a> -->
                </div>
                </li>

                <router-link to="/membertable" style="text-decoration: none;" v-if="isManager" v-show="isManager == true">
                  <li class="nav-item"><a class="nav-link active">MemberAll</a></li>
                </router-link>
            </ul>
              <div class="btn-group" role="group" aria-label="Basic example">
                <button type="button" class="btn btn-secondary" v-if="isToken" v-show="isToken == true" @click="[logout(), this.$router.push('/')]">로그아웃</button>
                <router-link to="/loginpage" v-else><button type="button" class="btn btn-secondary" v-show="isToken == false">로그인</button></router-link>
                <router-link to="/mypageinfo" v-if="isToken"><button type="button" class="btn btn-secondary" v-show="isToken == true">마이페이지</button></router-link>
                <router-link to="/signuppage" v-else><button type="button" class="btn btn-secondary" v-show="isToken == false">회원가입</button></router-link>
              </div>
            </div>
            </div>
            </nav>
          </div>
        </div>
      </div>
  </header>
  <!-- <button @click="getData">테스트</button>
  <button @click="getDel">테스트(manager)</button> -->
  <BAWFooter/>
  <router-view/>  
</template>

<script>
import BAWFooter from './components/common/BAWFooter.vue';
import axios from 'axios';
axios.defaults.baseURL="http://ec2-13-125-131-254.ap-northeast-2.compute.amazonaws.com";

export default {
  name: "App",
  components: {
    BAWFooter
  },
  computed : {
		isToken() {
      if(localStorage.getItem("token")) {
        return true;
      }else {
        return false;
      }
		},
    isManager() {
      if(localStorage.getItem("ismanager") === "true") {
        return true;
      }else {
        return false;
      }
    },
  },
  methods: {
    logout() {
      localStorage.removeItem("token");
      localStorage.removeItem("idx");
      localStorage.removeItem("ismanager");
      this.$router.go();
      this.$router.push('/');

    }
    // getData() {
    //   axios.get('/members/all')
    //   .then(res => {
    //     res.data.list.forEach(item => console.log(item))
    //     // const data = res.data
    //     // this.products = data
    //   })
    //   .catch(error => console.log(error))
    // },
    // getDel() {
    //   axios.get('/posts/del')
    //   .then(res => {
    //     res.data.list.forEach(item => console.log(item))
    //     // const data = res.data
    //     // this.products = data
    //   })
    //   .catch(error => console.log(error))
    // },
  },
};
</script>

<style>

#app {
  min-height: 100%;
  position: relative;
  background-color: #1a0933;
  padding-bottom: 200px;
  text-decoration: none;
}

</style>
