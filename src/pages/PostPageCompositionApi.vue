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
            />
        </my-dialog>        
        <post-list 
            :posts="sortedAndSearchedPosts"
            v-if="!isPostsLoading"
        />
        <div v-else>Идет загрузка...</div>
    </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import {usePosts} from "@/hooks/usePosts";
import useSortedPosts from "@/hooks/useSortedPosts";
import useSortedAndSearchedPosts from "@/hooks/useSortedAndSearchedPosts";

    export default {
        components: {
            PostList,
            PostForm,
        },
        data() {
            return {            
                dialogVisible: false,
                sortOptions: [
                    {value: 'title', name: 'по названию'},
                    {value: 'body', name: 'по содержимому'},
                ],
            }
        },
        setup(props) {
            const {posts, totalPages, isPostsLoading} = usePosts(10);
            const {sortedPosts, selectedSort} = useSortedPosts(posts);
            const {searchQuery, sortedAndSearchedPosts} = useSortedAndSearchedPosts(sortedPosts);

            return {
                posts,
                totalPages,
                isPostsLoading,
                sortedPosts, 
                selectedSort,
                searchQuery, 
                sortedAndSearchedPosts
            }
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