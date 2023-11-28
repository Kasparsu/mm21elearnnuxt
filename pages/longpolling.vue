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
        let res = await axios.get('http://localhost:3001/api/messages');
        this.messages.push(...res.data);
        
        let date = new Date().toISOString();
        while(true){
            res = await axios.get('http://localhost:3001/api/messages/long?from=' + date)
            this.messages.push(...res.data);
            console.log(res.data != []);
            if(res.data.length){
                date = new Date().toISOString();
            }
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