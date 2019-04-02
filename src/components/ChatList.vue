<template>
    <div id="chat-list">
        <ChatMessage v-for="(m, index)  in messages" :key="index" :message="m.msg" :right="m.right" :first="m.first"
                     :end="m.end"></ChatMessage>
    </div>
</template>

<script>
    import ChatMessage from "./ChatMessage";

    export default {
        name: "ChatList",
        components: {ChatMessage},
        data() {
            return {
                messages: [],
                questions: [{
                    id: 1,
                    messages: [{
                        msg: "嗨️",
                        first: true
                    }, {
                        msg: "欢迎来寻找失去的东西",
                    }, {
                        msg: "这是真实测试",
                    }, {
                        msg: "开始测试之前",
                    }, {
                        msg: "要不要先了解一下花花？",
                        end: true
                    }], // 消息
                    answers: [{
                        id: 11,
                        content: "了解一下",
                    }, {
                        id: 12,
                        content: "直接答题",
                    }], //答案
                }, {
                    id: 2,
                    messages: [{
                        msg: "花花",
                        first: true
                    }, {
                        msg: "花花花花花花花花花花花花",
                    }, {
                        msg: "花花花花花花花花花花花花花花花花花",
                    }, {
                        msg: "花花",
                    }, {
                        msg: "啦啦啦",
                        end: true
                    }], // 消息
                    answers: [{
                        id: 12,
                        content: "直接答题",
                    }], //答案
                }, {
                    id: 3,
                    messages: [{
                        msg: "ok",
                        first: true
                    }, {
                        msg: "1+1=?",
                    }, {
                        msg: "=2",
                    }, {
                        msg: "没猜对吧",
                    }, {
                        msg: "懂规则了吗？",
                        end: true
                    }], // 消息
                    answers: [{
                        id: 13,
                        content: "好的，我明白了",
                    }], //答案
                }],
                scrollToBottomTimeId: undefined
            }
        },
        created() {
            this.$log.debug('messages', this.messages);
            // 开始第一个问题
            this.initQuestionMessages(0);
            this.$bus.$on("answer", (res) => {
                this.$log.debug("bus answer", res);
                // 添加回答
                let message = {
                    msg: res.content,
                    right: true,
                };
                this.messages.push(message);
                this.scrollToBottom();
                if (res.id === 11) {
                    this.initQuestionMessages(1);
                }
                if (res.id === 12) {
                    this.initQuestionMessages(2);
                }
            })
        },
        beforeDestroy() {
            this.$bus.$off("answer")
        },
        methods: {
            //滚动条滚动到底部
            scrollToBottom: function () {
                if (this.scrollToBottomTimeId) {
                    clearTimeout(this.scrollToBottomTimeId);
                }
                this.scrollToBottomTimeId = setTimeout(function () {
                    const chat = document.getElementById('chat-list');
                    chat.scrollTop = chat.scrollHeight;
                }, 100);
            },
            initQuestionMessages: function (id) {
                const msgs = this.questions[id].messages;
                if (msgs) {
                    const that = this;
                    let i = 0;
                    const iv = setInterval(() => {
                        if (i < msgs.length) {
                            that.messages.push(msgs[i]);
                            this.scrollToBottom();
                            i++;
                        } else {
                            clearInterval(iv);
                            const ans = this.questions[id].answers;
                            if (ans) {
                                this.$bus.$emit("action", ans)
                            }
                            //事件广播
                        }
                    }, 1000)
                }
            }
        }
    }
</script>

<style scoped>
    #chat-list {
        height: calc(100% - 1.6rem);
        width: 100%;
        overflow-y: auto;
    }

    #chat-list::-webkit-scrollbar {
        display: none;
    }
</style>
