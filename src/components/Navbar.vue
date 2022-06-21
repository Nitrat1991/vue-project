<template>
    <div class="navbar">
        <div @click="$router.push('/')">Vue 3</div>
        <div class="navbar__btns">
            <my-button @click="$router.push('/posts')">Посты</my-button>
            <my-button @click="$router.push('/about')">О сайте</my-button>
            <my-button @click="$router.push('/store')">Store</my-button>
            <my-button @click="$router.push('/composition')">Composition</my-button>
        </div>
    </div>
</template>

<script>
    export default {
      name: 'navbar',
    }
</script>

<style scoped>
.navbar {
    height: 50px;
    background-color: rgb(207, 207, 207);
    box-shadow: 2px 2px 4px gray;
    display: flex;
    align-items: center;
    padding: 10px 15px;
}

.navbar__btns {
    margin-left: auto;
}

.navbar__btns button:not(:first-child) {
    margin-left: 20px;
}
</style>