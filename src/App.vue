<template>
    <div class="app">
        <h1>Страница с постами</h1>
        <my-button
            @click="showDialog"
            style="margin: 15px 0;"
        >
            Создать пост
        </my-button>
        <my-dialog v-model:show="dialogVisible">
            <post-form
                @create='creatPost'
            />
        </my-dialog>        
        <post-list 
            :posts="posts"
            @remove="removePost"
            v-if="!isPostsLoading"
        />
        <div v-else>Идет загрузка...</div>
    </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import axios from 'axios';

    export default {
        components: {
    PostList,
    PostForm,
},
        data() {
            return {
                posts: [],
                dialogVisible: false,
                modificatorValue: '',
                isPostsLoading: false,
            }
        },
        methods: {
            creatPost(post) {
                this.posts.push(post);
                this.dialogVisible = false;
            },
            removePost(post) {
                this.posts = this.posts.filter(p => p.id !== post.id);
            },
            showDialog() {
                this.dialogVisible = true;
            },
            async fetchPosts() {
                try{
                    this.isPostsLoading = true;                   
                    const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10');
                    this.posts = response.data;
                } catch (e) {
                    alert('Ошибка')
                } finally {
                    this.isPostsLoading = false;
                }
            }

        },
        mounted() {
            this.fetchPosts();
        }
        
    }
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-bos;
}

.post {
    padding: 15px;
    border: 2px solid green;
    margin-top: 15px;
}

.app {
    padding: 20px;
}

form {
    display: flex;
    flex-direction: column;

}

.input {
    border: 2px solid teal;
    padding: 18px 15px;
    margin-top: 10px;
}
</style>