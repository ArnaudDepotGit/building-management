<template>
  <div class="room border border-secondary rounded p-2 mb-2" :class="{expanded: isExpanded}">
    <div class="top-row d-flex" @click="toggleExpand">
      <div class="room-name fw-bold pe-3">{{room.name}}</div>
      <div class="building-id text-muted">Building{{room.buildingId}}</div>

      <div class="expand-button ms-auto">
        {{ isExpanded ? '&#9660;' : '&#9658;' }}
      </div>
    </div>
    <template v-if="isExpanded">
      <hr/>
      <div class="details d-flex">
        <button type="button" class="btn btn-danger" @click="deleteRooms">Delete room</button>
      </div>
    </template>
  </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';

export default {
  name: 'RoomsListItem',
  props: ['room'],
  data: function() {
    return {
      isExpanded: false
    }
  }, 
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async deleteRooms() {
      let response = await axios.get(`${API_HOST}/api/rooms/${this.room.id}`);
      let deletedRoom = response.data;
      this.$emit('room-deleted', deletedRoom);
      await axios.delete(`${API_HOST}/api/rooms/${this.room.id}`);
      ;
    }
  }
}
</script>

<style lang="scss" scoped>

.room {
  .top-row {
    cursor: pointer;
  }
}
</style>
