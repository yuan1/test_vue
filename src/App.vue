<template>
    <div id="app">
        <ChatList></ChatList>
        <ChatAction></ChatAction>
    </div>
</template>

<script>
    import ChatList from "./components/ChatList";
    import ChatAction from "./components/ChatAction";

    export default {
        name: 'app',
        components: {
            ChatAction,
            ChatList,
        }
    }
</script>

<style>
    * {
        margin: 0;
        padding: 0;
    }

    html,
    body {
        height: 100%;
        background: #f1f1f1;
    }

    #app {
        width: 100%;
        height: 100%;
    }
</style>
