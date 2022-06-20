<template>
    <button class="btn">
        <slot></slot>
    </button>
</template>

<script>
    export default {
      name: 'my-button',
    }
</script>

<style scope>
.btn { 
    align-self: flex-end;
    padding: 18px 15px;
    background: none;
    color: teal;
    border: 1px solid teal;
}
</style>