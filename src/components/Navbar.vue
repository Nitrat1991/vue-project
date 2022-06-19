<template>
    <div class="navbar">
        <div>Vue 3</div>
        <div class="navbar__btns">
            <my-button>Посты</my-button>
            <my-button>О сайте</my-button>
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
    background-color: gray;
    box-shadow: 2px 2px 4px gray;
    display: flex;
    align-items: center;
}
</style>