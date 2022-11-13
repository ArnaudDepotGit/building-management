<template>
    <div class="windows-form pt-3">
      <h4>Add a new window :</h4>
        <form id="myForm">
            <label for="windowName" class="info">Name of the new window :</label>
            <input class="form info" id="windowName" name="windowName" value="" placeholder="Window x" required>

            <span>Windows Status :</span>
            <input type="radio" class="radio form-check-input" id="openStatus" name="windowStatus" value="OPEN" checked>
            <label for="windowOpen" class="radio form-check-label">Open</label>
            <input type="radio" class="radio form-check-input" id="closedStatus" name="windowStatus" value="CLOSED">
            <label for="windowClosed" class="radio form-check-label">Close</label>
            
            <label for="room" class="info">Name of the room :</label>
            <select class="info" id="selectRoom" name="room" form="myForm" @click.once="selectRoom" required></select>
            
            <input type="submit" class="submit btn btn-primary" value="Create" @click="submitForm">
        </form>
    </div>
  </template>


<script>
import axios from 'axios';
import {API_HOST} from '../config';

export default {
  name: 'WindowsForm',
  props: ['window'], 
  methods: {
    async selectRoom() {
      let select = document.getElementById("selectRoom");

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

      rooms.forEach(room => {
        let newOption = document.createElement("option");
        newOption.value = room.id;
        newOption.textContent = room.name + " in building " + room.buildingId;
        select.appendChild(newOption);
      }); 
    },
    async submitForm() {
        let form = document.getElementById("myForm");
        await axios.post(`${API_HOST}/api/windows`, {
          id: null,
          name: form.childNodes[1].value,
          roomId: form.childNodes[8].value,
          roomName: null,
          windowStatus: form.childNodes[3].value
        })
    }
  }

}
</script>

<style lang="scss" scoped>
  h4 {
    text-align: center;
    margin-bottom: 0.5em;
  }
  label.info {
    width: 50%;
  }
  input.info, select.info {
    width: 40%;
    margin-right: 10%;
    text-align: center;
    height: 28px;
  }
  input.radio {
    margin-left: 10%;
  }
  label.radio {
    padding-left: 5px;
  }
  label {
    margin-bottom: 10px;
  }
  .submit {
    width: 120px;
    margin-top: 10px;
  }
</style>