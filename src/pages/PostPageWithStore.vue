<template>
    <div>
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
        <div v-intersection="loadMorePosts" class="observer"></div>
        <!-- <div class="page__wrapper">
            <div 
                v-for="pageNumber in totalPage" 
                :key="pageNumber"
                class="page"
                :class="{
                    'current__page': page === pageNumber
                }"
                @click="changePage(pageNumber)"    
            >
                {{ pageNumber }}
            </div>
        </div> -->
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
                page: 1,
                limit: 10,
                totalPage: 0,
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
            // changePage(pageNumber) {
            //     this.page = pageNumber;
            // },
            async fetchPosts() {
                try{
                    this.isPostsLoading = true;                   
                    const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                        params: {
                            _page: this.page,
                            _limit: this.limit,
                        }
                    });
                    this.totalPage = Math.ceil(response.headers['x-total-count'] / this.limit);
                    this.posts = response.data;
                } catch (e) {
                    alert('Ошибка')
                } finally {
                    this.isPostsLoading = false;
                }
            },
            async loadMorePosts() {
                try{
                    this.page += 1;             
                    const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                        params: {
                            _page: this.page,
                            _limit: this.limit,
                        }
                    });
                    this.totalPage = Math.ceil(response.headers['x-total-count'] / this.limit);
                    this.posts = [...this.posts, ...response.data];
                } catch (e) {
                    alert('Ошибка')
                }
            }

        },
        mounted() {
            this.fetchPosts();
            this.$refs.observer;
            // const options = {
            //     rootMargin: '0px',
            //     threshold: 1.0
            // }
            // const callback = (entries, observer) => {
            //     if (entries[0].isIntersecting && this.page < this.totalPage) {
            //         this.loadMorePosts();
            //     }
            // };
            // const observer = new IntersectionObserver(callback, options);
            // observer.observe(this.$refs.observer);
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
            // page() {
            //     this.fetchPosts();
            // }
        }
    }
</script>

<style scoped>
.post {
    padding: 15px;
    border: 2px solid green;
    margin-top: 15px;
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

.page__wrapper {
    display: flex;
    margin-top: 15px;
}

.page {
    border: 1px solid #000;
    padding: 10px;
}

.current__page {
    border: 2px solid teal;
}
</style>