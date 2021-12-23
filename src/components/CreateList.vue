<template>
    <div class="create border border-secondary rounded shadow-sm p-2 mb-2 mt-4 bg-white expanded">
        <div class="top-row d-flex">
            <div class="create-window fw-bold pe-3">Create window</div>
        </div>
        <hr/>
        <form>
        <input type="text" class="form-control form-control mb-2" v-model="windowName">

        <template>
            <div v-for="room in rooms" v-bind:room="room" v-bind:key="room.id"  class="form-check">
                <label class="form-check-label">
                    <input type="radio" class="form-check-input" name="roomOption" v-bind:value="room.id" v-model="roomNumber">{{room.name}}
                </label>
            </div>
        </template>

        <div class="details d-flex">
            <button type="button" class="addWindow btn p-2 btn-primary me-2" v-on:click="createWindow">Create window</button>
        </div>
        </form>
    </div>
</template>


<script>
import axios from 'axios';
import { API_HOST } from '../config';

export default {
    name: 'CreateList',
    data(){
        return {
            rooms: [],
            roomNumber:0,
            windowName:"",
        }
    },
    
    created: async function(){

            let response = await axios.get(`${API_HOST}/api/rooms`);
            this.rooms = response.data;
    },

    methods:{
        methodThatForcesUpdate() {
            this.$forceUpdate();
        },
        async createWindow(){
            let postData = {
                "name": this.windowName, 
                "roomId":this.roomNumber ,
                "windowStatus": "CLOSED"
            };
            await axios.post(`${API_HOST}/api/windows`, postData);
        },
    }

}
</script>

<style lang="scss" scoped>
.create{
    .top-row {
        cursor: pointer;
    }
}

.addWindow.btn, .addWindow.btn:active{
    color: white;
    background-color: #198754;
    border-color: #198754;
}

.create {
    .top-row {
        cursor: pointer;
    }
}
</style>