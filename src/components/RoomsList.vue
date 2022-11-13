<template>
  <div class="rooms-list pt-3">
    <rooms-list-item 
      v-for="room in rooms"
      :room="room"
      :key="room.id"  
      @room-deleted="deleteRoom"
    >
    </rooms-list-item>
    <rooms-form></rooms-form>
  </div>
</template>


<script>
import axios from 'axios';
import {API_HOST} from '../config';
import RoomsListItem from './RoomsListItem';
import RoomsForm from './RoomsForm.vue';


export default {
  components: {
    RoomsListItem,
    RoomsForm
  },
  name: 'PanelRooms',
  data: function() {
    return {
      /* Initialize windows with an empty array, while waiting for actual data to be retrieved from the API */
      rooms: []
    }
  },
  created: async function() {
    let response = await axios.get(`${API_HOST}/api/rooms`, {
      method: 'GET',
      mode: 'no-cors',
      headers: {
        'Access-Control-Allow-Origin': '*',
        'Content-Type': 'application/json',
      },
      auth: {
        username: 'admin',
        password: 'adminPassword'
      }
  });
    let rooms = response.data;
    this.rooms = rooms;
  },
  methods: {
    deleteRoom(newRoom){
      let index = this.rooms.findIndex(room => room.id === newRoom.id);
      this.rooms.splice(index, 1);
    }
  }
}
</script>
