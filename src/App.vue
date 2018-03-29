<template>
    <div id="app">
        <left-nav></left-nav>
        <router-view></router-view>
    </div>
</template>

<script>
    import leftNav from '@/components/leftNav';
    export default {
        name: 'App',
        components: {
            leftNav
        }
    }

</script>

<style>
    html,
    body,
    #app {
        height: 100%;
    }
    #app {
        font-family: 'microsoft yahei' ,'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        /* margin-top: 60px; */
    }
    /*.main {
        float: left;
        width: 95%;
        background-color: #eff2f7;
        overflow: hidden;
    }*/

</style>
