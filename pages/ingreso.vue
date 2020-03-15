<template>
  <div class="containerp">
    <div class="container1row">
      <b-card
        title="Ingreso de vehiculos"
        img-src="https://www.solojoomla.com/wp-content/uploads/2017/01/formularios.jpg"
        img-alt="Image"
        img-top
        tag="article"
        style="max-width: 30rem;"
        class="mb-2"
      >
        <b-card-text>
          Rellena todos los campos necesarios para ingresar
          un vehiculo al sistema (todos los campos son obligatorios).
        </b-card-text>

        <b-form @submit="onSubmit" @reset="onReset" v-if="show">
          <b-form-group id="input-group-1" label="Tipo de vehiculo:" label-for="input-1">
            <b-form-select id="tipoVehiculo" v-model="form.tipo" :options="tipo" required></b-form-select>
          </b-form-group>

          <b-form-group
            id="input-group-2"
            label="Placa:"
            label-for="input-2"
            description="En caso de las bicicletas la placa es el id del usuario."
          >
            <b-form-input id="placa" v-model="form.placa" required placeholder="Ingresa la placa"></b-form-input>
          </b-form-group>

          <b-form-group id="input-group-3" label="Color:" label-for="input-3">
            <b-form-input
              id="color"
              v-model="form.color"
              required
              placeholder="Ingresa el color del vehiculo"
            ></b-form-input>
          </b-form-group>

          <b-form-group id="input-group-4" label="Marca:" label-for="input-4">
            <b-form-input
              id="marca"
              v-model="form.marca"
              required
              placeholder="Ingresa la marca del vehiculo"
            ></b-form-input>
          </b-form-group>

          <b-form-group id="input-group-5" label="Ciudad de la placa:" label-for="input-5">
            <b-form-input
              id="cPlaca"
              v-model="form.cPlaca"
              required
              placeholder="Ingresa ciudad de la placa"
            ></b-form-input>
          </b-form-group>

          <div>
            <label for="example-datepicker">Selecciona un dia</label>
            <b-form-datepicker id="fecha" v-model="fecha" class="mb-2"></b-form-datepicker>
            <p>Fecha seleccionada: '{{ fecha }}'</p>
          </div>

          <div>
            <b-form-timepicker id="hora" v-model="hora" locale="en"></b-form-timepicker>
            <div class="mt-2">Hora seleccionada: '{{ hora }}'</div>
          </div>

          <b-button @click="ingresarVehiculo()" type="submit" variant="primary">Aceptar</b-button>
          <b-button @click="onReset()" type="reset" variant="danger">Limpiar</b-button>
        </b-form>
      </b-card>
    </div>

    <div class="container2">
      <center>
        <h1>Listado de vehiculos</h1>
      </center>

      <div class="container2row">
        <b-table striped hover :items="vehiculos" style="width:100%">
          <template v-slot:cell(acciones)="row">
            <!-- Botones para editar y eliminar aplicaciones de la lista -->
            <b-button size="sm" @click="cargarAplicacion(row)" class="mr-2">Modificar</b-button>
            <b-button
              size="sm"
              @click="cargarAplicacion(row)"
              class="mr-2"
              variant="danger"
            >Eliminar</b-button>
            <b-button
              size="sm"
              @click="cargarAplicacion(row)"
              class="mr-2"
              variant="success"
            >Dar Salida</b-button>
          </template>
        </b-table>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {

      fecha: "",
      hora: "",
      items: [
        { age: 40, first_name: "Dickerson", last_name: "Macdonald" },
        { age: 21, first_name: "Larsen", last_name: "Shaw" },
        { age: 89, first_name: "Geneva", last_name: "Wilson" },
        { age: 38, first_name: "Jami", last_name: "Carney" }
      ],

      vehiculo: {
        tipo: null,
        placa: "",
        color: "",
        marca: "",
        cPlaca: "",
        fecha: new Date(),
        hora: new Date(),
        acciones: true
      },

      vehiculos: [
        {
          tipo: "Moto",
          placa: "VUP09D",
          color: "Negro",
          marca: "AKT",
          cPlaca: "Medellin",
          fecha: "01/13/20",
          hora: "00:00:00",
          acciones: true
        }
      ],

      form: {
        email: "",
        name: "",
        tipo: null,
        checked: []
      },

      tipo: [
        { text: "Selecciona uno", value: null },
        "Carro",
        "Moto",
        "Bicicleta",
        "Camion"
      ],
      show: true
    };
  },
  methods: {
    ingresarVehiculo() {
      this.vehiculo = {
        tipo: document.getElementById("tipoVehiculo").value,
        placa: document.getElementById("placa").value,
        color: document.getElementById("color").value,
        marca: document.getElementById("marca").value,
        cPlaca: document.getElementById("cPlaca").value,
        fecha: document.getElementById("fecha").date(),
        hora: document.getElementById("hora").value,
        acciones: true
      };
      this.vehiculos.push(this.vehiculo);
      /*Convierte el array aplicaciones en un JSON y lo almacena en el localStorage*/
      var jsonPerson = JSON.stringify(this.vehiculos);
      localStorage.setItem("vehiculostorage", jsonPerson);
    },

    onSubmit(evt) {
      evt.preventDefault();
      alert(JSON.stringify(this.vehiculo));
    },
    onReset(evt) {
      evt.preventDefault();
      // Reset our form values
      this.form.placa = "";
      this.form.color = "";
      this.form.tipo = null;
      this.form.checked = [];
      // Trick to reset/clear native browser form validation state
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    }
  }
};
</script>

<style>
.containerp {
  display: flex;
}
</style>