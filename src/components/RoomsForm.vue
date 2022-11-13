<template>
    <div class="rooms-form pt-3">
      <h4>Add a new room :</h4>
        <form id="myRoomForm">
            <label for="roomName" class="info">Name of the new room :</label>
            <input class="form info" id="roomName" name="roomName" value="" placeholder="Room x" required>
            
            <label for="floorName" class="info">Floor number :</label>
            <input class="form info" id="floorName" name="floorName" value="" required>

            <label for="building" class="info">Name of the building : </label>
            <select name="building" class="info" id="selectBuilding" form="myRoomForm" @click.once="selectBuilding" required></select>
            
            <input type="submit" class="submit btn btn-primary" value="Create" @click="submitForm">
        </form>
    </div>
  </template>


<script>
import axios from 'axios';
import {API_HOST} from '../config';

export default {
  name: 'RoomsForm',
  props: ['room'], 
  methods: {
    async selectBuilding() {
      let select = document.getElementById("selectBuilding");

      let response = await axios.get(`${API_HOST}/api/buildings`, {
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
    let buildings = response.data;

      buildings.forEach(building => {
        let newOption = document.createElement("option");
        newOption.value = building.id;
        newOption.textContent = "Building :" + building.id;
        select.appendChild(newOption);
      }); 
    },
    async submitForm() {
        let form = document.getElementById("myRoomForm");
        await axios.post(`${API_HOST}/api/rooms`, {
          buildingId: form.childNodes[5].value,
          currentTemperature: null,
          floor: form.childNodes[3].value,
          id: null,
          name: form.childNodes[1].value,
          targetTemperature: null
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
  label {
    margin-bottom: 10px;
  }
  .submit {
    width: 120px;
    margin-top: 10px;
  }
</style>