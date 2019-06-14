<template>
  <div>
    <div class="container">
      <div class="row">
        <div class="col-md-6">
          <b-form @submit="onSubmit" v-if="show">
            <b-form-group>
              <b-form-input
                id="input-1"
                v-model="form1.search"
                type="text"
                required
                placeholder="Busque una imagen"
              ></b-form-input>
            </b-form-group>
            <b-button type="submit" variant="primary">Submit</b-button>
          </b-form>
        </div>
        <div class="col-md-6">
          <b-form @submit="onImage" v-if="show">
            <b-form-group>
              <b-form-input
                id="input-1"
                v-model="form2.search"
                type="text"
                required
                placeholder="Busque una imagen"
              ></b-form-input>
            </b-form-group>
            <b-button type="submit" variant="primary">Submit</b-button>
          </b-form>
          <div class="col-md-6 align-center">
            <img :src="'http://localhost/TallerDistribuido/public'+image" alt="" class="img-fluid">
          </div>
        </div>
        <div class="col-md-12">
          <b-form @submit="storeImage" v-if="show">
          <b-form-group>
              <b-form-input
                id="input-1"
                v-model="form3.name"
                type="text"
                required
                placeholder="Dale nombre a tu imagen"
              ></b-form-input>
            </b-form-group>
            <b-form-group>
              <b-form-file
                v-model="file"
                :state="Boolean(file)"
                placeholder="Choose a file..."
                drop-placeholder="Drop file here..."
                accept=".jpg"
              ></b-form-file>
            </b-form-group>
            <b-button type="submit" variant="primary">Submit</b-button>
          </b-form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import axios from 'axios'

export default {
  data () {
    return {
      form1: {
        search: ''
      },
      form2: {
        search: ''
      },
      form3: {
        name: ''
      },
      image: '',
      file: null,
      show: true
    }
  },
  methods: {
    onSubmit (evt) {
      evt.preventDefault()
      axios.get('http://localhost/TallerDistribuido/public/api/v1/image/search/' + this.form1.search)
        .then(function (resp) {
          alert(resp.data)
          this.form1.search = ''
        }).catch(function (error) {
          console.log('an error occured ' + error)
        })
    },
    onImage (evt) {
      let app = this
      evt.preventDefault()
      axios.get('http://localhost/TallerDistribuido/public/api/v1/image/get/' + this.form2.search)
        .then(function (resp) {
          app.image = resp.data.path
          app.form2.search = ''
        }).catch(function (error) {
          console.log('an error occured ' + error)
        })
    },
    storeImage () {
      let img = this.file
      let fd = new FormData()

      fd.append('image', img)
      fd.append('name', this.form3.name)

      axios.post('http://localhost/TallerDistribuido/public/api/v1/image/insert', fd)
        .then(function (resp) {
          alert(resp.data)
        }).catch(function (error) {
          console.log('an error occured ' + error)
        })
    }
  }
}
</script>

<style scoped>

</style>
