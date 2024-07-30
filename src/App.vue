<template>
  <div id="app">
    <h1>Sheet Data Processor</h1>
    <form @submit.prevent="processData">
      <div>
        <label for="srcSheet">Source Sheet:</label>
        <input v-model="srcSheet" id="srcSheet" type="text" />
      </div>
      <div>
        <label for="desSheet">Destination Sheet:</label>
        <input v-model="desSheet" id="desSheet" type="text" />
      </div>
      <div>
        <h2>Source Sheet Data</h2>
        <div v-for="(item, index) in srcSheetData" :key="index">
          <input type="checkbox" :id="'src' + index" v-model="selectedSrcData" :value="item" />
          <label :for="'src' + index">{{ item }}</label>
        </div>
      </div>
      <div>
        <h2>Destination Sheet Data</h2>
        <div v-for="(item, index) in desSheetData" :key="index">
          <input type="checkbox" :id="'des' + index" v-model="selectedDesData" :value="item" />
          <label :for="'des' + index">{{ item }}</label>
        </div>
      </div>
      <button type="button" @click="loadData">Load Data</button>
      <div>
        <h2>All Ranges from Destination Sheet</h2>
        <div v-for="(item, index) in allRangesData" :key="index">
          <input type="checkbox" :id="'range' + index" v-model="selectedRanges" :value="item" />
          <label :for="'range' + index">{{ item }}</label>
        </div>
      </div>
      <button type="submit">Process</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      srcSheet: null,
      desSheet: null,
      srcSheetData: [],
      desSheetData: [],
      selectedSrcData: [],
      selectedDesData: [],
      allRangesData: [],
      selectedRanges: [],
      // other data properties
    };
  },
  watch: {
    srcSheet(newVal) {
      if (newVal) {
        this.fetchSrcSheets();
      }
    },
    desSheet(newVal) {
      if (newVal) {
        this.fetchDesSheets();
      }
    }
  },
  methods: {
    async fetchSrcSheets() {
      try {
        const response = await axios.post('http://127.0.0.1:8000/api/core/getAllSheetsFromSpreadsheet', {
          src_sheet_url: this.srcSheet
        });
        this.srcSheetData = response.data.srcSheetData || [];
      } catch (error) {
        console.error('Error fetching source sheets:', error);
      }
    },
    async fetchDesSheets() {
      try {
        const response = await axios.post('http://127.0.0.1:8000/api/core/getAllSheetsFromSpreadsheet', {
          des_sheet_url: this.desSheet
        });
        this.desSheetData = response.data.desSheetData || [];
      } catch (error) {
        console.error('Error fetching destination sheets:', error);
      }
    },
    loadData() {
      // Load data logic here
      console.log('Load data clicked');
    },
    processData() {
      // Process the selected sheets
      console.log('Selected Source Sheets:', this.selectedSrcData);
      console.log('Selected Destination Sheets:', this.selectedDesData);
      console.log('Selected Ranges:', this.selectedRanges);
    }
  }
};
</script>


<style>
#app {
  font-family: Arial, sans-serif;
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

h1, h2 {
  color: #333;
}

form {
  display: flex;
  flex-direction: column;
}

form > div {
  margin-bottom: 20px;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

input[type="text"] {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

input[type="checkbox"] {
  margin-right: 10px;
}

button {
  padding: 10px 15px;
  background-color: #007BFF;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

div > div {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

@media (max-width: 600px) {
  #app {
    padding: 10px;
  }

  button {
    width: 100%;
  }
}
</style>
