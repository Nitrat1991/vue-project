<template>
    <div class="app">
        <h1>Страница с постами</h1>
        <my-input
            v-model="searchQuery"
            placeholder="Поиск...."
        />
        <div class="app__btns">
            <my-button
            @click="showDialog"
            >
                Создать пост
            </my-button>
            <my-select
                v-model="selectedSort"
                :options="sortOptions"
            />
        </div>
        
        <my-dialog v-model:show="dialogVisible">
            <post-form
                @create='creatPost'
            />
        </my-dialog>        
        <post-list 
            :posts="sortedAndSearchedPosts"
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
                selectedSort: '',
                searchQuery: '',
                sortOptions: [
                    {value: 'title', name: 'по названию'},
                    {value: 'body', name: 'по содержимому'},
                ],
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
        },
        computed: {
            sortedPosts() {
                return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
                )
            },
            sortedAndSearchedPosts() {
                return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
            }
        },
        watch: {
            // selectedSort(newValue) {
            //     this.posts.sort((post1, post2) => {
            //         return post1[newValue]?.localeCompare(post2[newValue]);
            //     })
            // },
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

.app__btns {
    display: flex;
    justify-content: space-between;
    margin: 15px 0;
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