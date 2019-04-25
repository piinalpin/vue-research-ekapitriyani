<template>
  <div class="animated fadeIn">
    <notifications group="errorMessage" position="top right" animation-type="velocity" style="top:20px;"/>
    <br><br>
    <b-row class="justify-content-center">
      <b-col lg="6">
        <b-form-group>
          <img src="img/Research-transparent.png" class="w-75 mx-auto d-block">
          <b-input-group class="mt-3">
            <b-form-input class="form-control-lg" type="text" placeholder="Input Query" v-model="query"></b-form-input>
            <b-input-group-append><b-input-group-text><i class="fa fa-search"></i></b-input-group-text></b-input-group-append>
          </b-input-group>
        </b-form-group>
        <h5 class="text-center">--- OR ---</h5>
        <div class="input-group mb-3">
          <div class="custom-file">
            <input type="file" ref="file" v-on:change="fileHandler()" class="custom-file-input" id="file">
            <label class="custom-file-label form-control-lg" for="file">Choose file .xlsx</label>
          </div>
        </div>
      </b-col>
    </b-row>
    <b-row class="justify-content-center">
      <b-col lg="6">
        <b-row class="justify-content-end">
          <b-col md="4">
            <b-button block v-on:click="sendRequest" variant="primary">Send Request</b-button>
          </b-col>
        </b-row>
      </b-col>
    </b-row>
    <br>
    <b-row>
      <b-col lg="12">
        <b-card v-if="items" style="width:100%">
          <table class="table table-responsive">
            <thead>
              <tr>
                <th colspan="2" style="background-color: #20a8d8; color:white;">Query</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in items">
                <td><b>{{ item.query }}</b></td>
                <td>
                  <table class="table table-hover table-borderless">
                    <thead>
                      <tr>
                        <th>Judul</th>
                        <th>Pembimbing</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="detail in item.details">
                        <td> {{ detail.judul }} </td>
                        <td> {{ detail.pembimbing }} </td>
                      </tr>
                    </tbody>
                  </table>
                </td>
              </tr>
            </tbody>
          </table>
        </b-card>
      </b-col>
    </b-row>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'dashboard',
  data: () => {
    return {
      query: '',
      file: null,
      items: null
    }
  },
  methods: {
    sendRequest() {
      if (this.query != "" && this.file == null) {
        axios.get(process.env.VUE_APP_ROOT_API + "/search?q=" + this.query).then((response) => {
          this.items = response.data;
        })
      } else if (this.query == "" && this.file != null) {
        let formData = new FormData();
        formData.append('files', this.file);
        axios.post(process.env.VUE_APP_ROOT_API + "/search", formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        }).then((response) => {
          this.items = response.data;
        }).catch(e => console.log(e.response.data))
      } else {
        this.$notify({
          group: "errorMessage",
          type: "error",
          text: "request not found or can not double request",
          duration: 5000
        });
      }
    },
    fileHandler() {
        this.file = this.$refs.file.files[0];
    }
  },
  beforeMount() {
    this.file = null
  }
}
</script>

<style>
  /* IE fix */
  #card-chart-01, #card-chart-02 {
    width: 100% !important;
  }
</style>
