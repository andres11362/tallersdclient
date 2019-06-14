<template>
  <div>
    <div class="container">
      <div class="row">
        <div class="col-md-6">
          <h1>Existe la imagen?</h1>
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
          <h1>Vamos a buscarla</h1>
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
          <br>
          <div class="align-center" v-show="picture">
            <img :src="'http://tallersd.test'+image" alt="" class="img-fluid" width="300" height="500">
          </div>
        </div>
        <div class="col-md-12">
          <h1>Inserta tu imagen</h1>
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
    <notifications group="foo" position="top center" />
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
      show: true,
      picture: false
    }
  },
  methods: {
    onSubmit (evt) {
      let app = this
      evt.preventDefault()
      axios.get('http://tallersd.test/api/v1/image/search/' + app.form1.search)
        .then(function (resp) {
          app.$notify({
            group: 'foo',
            title: 'Alerta',
            text: resp.data,
            type: 'success'
          })
          app.form1.search = ''
        }).catch(function (error) {
          console.log('an error occured ' + error)
        })
    },
    onImage (evt) {
      let app = this
      evt.preventDefault()
      axios.get('http://tallersd.test/api/v1/image/get/' + app.form2.search)
        .then(function (resp) {
          app.$notify({
            group: 'foo',
            title: 'Alerta',
            text: 'Imagen cargada desde el servidor',
            type: 'success'
          })
          app.picture = true
          app.image = resp.data.path
          app.form2.search = ''
        }).catch(function (error) {
          console.log('an error occured ' + error)
        })
    },
    storeImage () {
      let app = this
      let img = app.file
      let fd = new FormData()

      fd.append('image', img)
      fd.append('name', app.form3.name)

      axios.post('http://tallersd.test/api/v1/image/insert', fd)
        .then(function (resp) {
          app.$notify({
            group: 'foo',
            title: 'Alerta',
            text: resp.data,
            type: 'success'
          })
          app.form3.name = ''
          app.file = ''
        }).catch(function (error) {
          console.log('an error occured ' + error)
        })
    }
  }
}
</script>

<style scoped>

</style>
