<template>
    <div class="bawbody">
        <form class="container h-100">
        <div class="card border-primary mb-3">
            <div class="card-header">자유게시판</div>
            <div class="card-body">
                <input class="form-control" v-model="post.title">
                <textarea class="form-control bawtextarea" v-model="post.content"></textarea>
            </div>
        </div>

        <div style="padding-top: 50px;">
            <div class="three">
                <router-link to="/"><button type="button" class="btn btn-primary three-button">수정</button></router-link>
                <router-link to="/"><button type="button" class="btn btn-primary three-button">삭제</button></router-link>
                <router-link to="/board"><button type="button" class="btn btn-primary three-button">목록</button></router-link>
            </div>
        </div>
        </form>
    </div>
</template>

<script>
import axios from 'axios';
axios.defaults.baseURL="http://ec2-13-125-131-254.ap-northeast-2.compute.amazonaws.com";

export default {
    name: 'PostUpdatePage',
    data() {        
        return {
            post: "",                
        }
    },
    created() {
        this.getData();
    },
    methods: {            
        getData() {
            axios.get('/posts/idx/' + this.$route.query.postIdx)
            .then(res => {
                this.post = res.data.data;                    
            })
            .catch(error => console.log(error));
        },
        deleted() {
            axios.patch('/posts/' + this.$route.query.postIdx)
            .then(res => {
                this.post = res.data.data;
            })
        },
        list(){
            this.$router.push({
                path: '/board'
            })
        }
    },    
    mounted() {
        this.getData();
    },
}
</script>

<style>
.bawtextarea {
    resize: none;
    height: 300px;
    margin-top: 5px;
}

.three {
  display: flex;
  justify-content: space-between;
}

.three-button {
  min-width: 140px;
}

</style>