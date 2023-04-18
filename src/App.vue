  // Inicio de la plantilla básica del formulario
<template>
  <div class="">
    <form @submit.prevent="submitForm">
      <label for="name">Nombre:</label>
      <input type="text" id="name" v-model="name" required>

      <label for="email">Correo electrónico:</label>
      <input type="email" id="email" v-model="email" required>

      <label for="country">País:</label>
      <select id="country" v-model="selectedCountry" @change="getCities">
        <option value="">Seleccionar país</option>
        <option v-for="country in countries" :value="country.name" :key="country.code">{{ country.name }}</option>
      </select>

      <label for="city">Ciudad:</label>
      <select id="city" v-model="selectedCity" :disabled="!cities.length">
        <option value="">Seleccionar ciudad</option>
        <option v-for="city in cities" :value="city.name" :key="city.name">{{ city.name }}</option>
      </select>

      <button type="submit">Enviar</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: '',
      email: '',
      selectedCountry: '',
      selectedCity: '',
      countries: [],
      cities: []
    }
  },
  methods: {
    getCountries() {
      fetch('https://restcountries.com/v2/all')
        .then(response => response.json())
        .then(data => {
          console.log(data);
          this.countries = data;
        })
        .catch(error => console.error(error))
    },
   getCities() {
  if (this.selectedCountry) {
    fetch(`https://restcountries.com/v2/name/${encodeURIComponent(this.selectedCountry)}?fullText=true`)
      .then(response => response.json())
      .then(data => {
        console.log(data);
        this.cities = data[0].capital ? [{ name: data[0].capital }] : [];
      })
      .catch(error => console.error(error))
  } else {
    this.cities = []
  }
},
    submitForm() {
      // Aquí se envian los datos del formulario a través de una petición AJAX
      console.log('Nombre:', this.name)
      console.log('Correo electrónico:', this.email)
      console.log('País seleccionado:', this.selectedCountry)
      console.log('Ciudad seleccionada:', this.selectedCity)
    }
  },
  mounted() {
    this.getCountries()
  }
}
</script>
  // Aquí se definen estilos
<style>
.form-container {
  background-color: white;
  box-shadow: 0px 0px 5px grey;
  padding-top: 80px;

}

.form {
  display: flex;
  flex-direction: column;
  gap: 20px;
 min-height:70vh;
  margin: 0 auto;
}

label {
  font-weight: bold;
  font-family: 'arial'

 
}

input, select {
  width: 100%;
  padding: 10px;
  border-radius: 5px;
  border: none;
  box-shadow: 0px 0px 5px grey;
}

.btn-submit {
  background-color: green;
  color: green;
  border: none;
  padding: 10px;
  border-radius: 5px;
  cursor: pointer;
  width: 100%;
}

.btn-submit:hover {
  opacity: 0.8;}
</style>

