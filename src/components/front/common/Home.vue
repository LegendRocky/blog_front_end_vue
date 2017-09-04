<template>
    <div class="article-wrapper">
        <v-head></v-head>
        <div class="front-content">
            <transition name="move" mode="out-in"><router-view></router-view></transition>
        </div>
        <v-footer></v-footer>
    </div>
</template>

<script>
    import vHead from './Header.vue';
    import vFooter from './Footer.vue';
    export default {
        components:{
            vHead,
            vFooter
        }
    }
</script>

<style scoped>
    .article-wrapper {
        background: rgb(240, 240, 240);
        min-height: 100%;
    }
</style>
