<template>
    <div>
      <h1>Create Rencana</h1>
      <form @submit.prevent="createRencana">
        <input v-model="rencanaData.rencana" type="text" placeholder="Rencana" required>
        <input v-model="rencanaData.waktu" type="text" placeholder="Waktu" required>
        <input v-model="rencanaData.keterangan" type="text" placeholder="Keterangan" required>
        <button type="submit">Create</button>
      </form>

      <h1>Edit Rencana</h1>
      <form @submit.prevent="updateRencana">
        <input v-model="selectedRencana.rencana" type="text" placeholder="Rencana" required>
        <input v-model="selectedRencana.waktu" type="text" placeholder="Waktu" required>
        <input v-model="selectedRencana.keterangan" type="text" placeholder="Keterangan" required>
        <button type="submit">Update</button>
      </form>
    </div>
  
  
      <h1>All Rencana</h1>
      <table>
        <thead>
          <tr>
            <th>Rencana</th>
            <th>Waktu</th>
            <th>Keterangan</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="rencana in rencanas" :key="rencana._id">
            <td>{{ rencana.rencana }}</td>
            <td>{{ rencana.waktu }}</td>
            <td>{{ rencana.keterangan }}</td>
            <td>
              <button @click="getRencanaById(rencana._id)">Edit</button>
              <button @click="deleteRencana(rencana._id)">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </template>
      
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        rencanaData: {
          rencana: '',
          waktu: '',
          keterangan: '',
        },
        rencanas: [],
        selectedRencana: {
          _id: '',
          rencana: '',
          waktu: '',
          keterangan: '',
        },
      };
    },
    mounted() {
      this.getAllRencana();
    },
    methods: {
      getToken() {
        return localStorage.getItem('token');
      },
      async createRencana() {
        try {
          const response = await axios.post('http://localhost:3333/data/rencana', this.rencanaData, {
            headers: { Authorization: `Bearer ${this.getToken()}` },
          });
          this.rencanas.push(response.data);
          this.rencanaData.rencana = '';
          this.rencanaData.waktu = '';
          this.rencanaData.keterangan = '';
        } catch (error) {
          console.error(error);
        }
      },
      async getAllRencana() {
        try {
          const response = await axios.get('http://localhost:3333/data/rencana', {
            headers: { Authorization: `Bearer ${this.getToken()}` },
          });
          this.rencanas = response.data;
        } catch (error) {
          console.error(error);
        }
      },
      async getRencanaById(id) {
        try {
          const response = await axios.get(`http://localhost:3333/data/rencana/${id}`, {
            headers: { Authorization: `Bearer ${this.getToken()}` },
          });
          this.selectedRencana = { ...response.data };
        } catch (error) {
          console.error(error);
        }
      },
      async updateRencana() {
        try {
          const response = await axios.put(
            `http://localhost:3333/data/rencana/${this.selectedRencana._id}`,
            this.selectedRencana,
            {
              headers: { Authorization: `Bearer ${this.getToken()}` },
            }
          );
          const index = this.rencanas.findIndex((rencana) => rencana._id === response.data._id);
          if (index !== -1) {
            this.rencanas.splice(index, 1, response.data);
          }
          this.selectedRencana = {
            _id: '',
            rencana: '',
            waktu: '',
            keterangan: '',
          };
        } catch (error) {
          console.error(error);
        }
      },
      async deleteRencana(id) {
        try {
          await axios.delete(`http://localhost:3333/data/rencana/${id}`, {
            headers: { Authorization: `Bearer ${this.getToken()}` },
          });
          this.rencanas = this.rencanas.filter((rencana) => rencana._id !== id);
        } catch (error) {
          console.error(error);
        }
      },
    },
  };
  </script>
  