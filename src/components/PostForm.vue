<template>
    <form @submit.prevent>
        <h2>Создание поста</h2>
        <my-input v-focus v-model='post.title' type="text" placeholder="Название"/>
        <my-input v-model='post.body' type="text" placeholder="Описание"/>
        <my-button style="margin-top:10px;" @click='creatPost' class="btn">Создать</my-button>
    </form>
</template>

<script>
export default {
    data() {
        return {
            post: {
                title: '',
                body: '',
            }
        }
    },
    methods: {
        creatPost() {
                this.post.id = Date.now();
                this.$emit('create', this.post)
                this.post = {
                    title: '',
                    body: '',
                }
        },
    }
}
</script>

<style scope>
form {
    display: flex;
    flex-direction: column;

}
</style>