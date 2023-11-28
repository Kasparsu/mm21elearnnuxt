<template>
    <div class="container">
        <div class="field has-addons">
            <div class="control is-expanded">
                <input class="input" type="text" v-model="message">
            </div>
            <div class="control">
                <button class="button is-info" @click="send">
                    Send
                </button>
            </div>
        </div>
        <ul>
            <li v-for="msg in messages">{{ msg.text }}</li>
        </ul>
    </div>
</template>
<script>
import axios from 'axios';
export default {
    async created(){
        if (process.client) {
            const evtSource = new EventSource("http://localhost:3001/api/messages/sse");
            evtSource.addEventListener('messages', event => {
                console.log(event);
                this.messages.push(...JSON.parse(event.data));
            });
        }
    },
    data() {
        return {
            message: '',
            messages: []
        }
    },
    methods: {
        send() {
            axios.post('http://localhost:3001/api/messages', {
                message: this.message
            }).then(res => {
                console.log(res.data);
                this.message = '';
            });
        }
    }
}
</script>