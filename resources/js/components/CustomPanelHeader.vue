<template>
    <div class="panelHeader">
        <button @click="clickHandler">Close</button>
    </div>
</template>
<script setup lang="ts">
    const childData = false;
    const clickHandler = ()=>{
        alert(true);
    };
</script>
<style>
    .panelHeader{
        width: 100%;
        height: 40px;

    }
    .panelHeader button{
        position: absolute;
        top: 0;
        right: 0;
    }
</style>
