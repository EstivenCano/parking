<template>
  <div class="containerp">
    <div class="container1row">
      <b-card
        title="Ingreso de vehiculos"
        img-src="https://www.solojoomla.com/wp-content/uploads/2017/01/formularios.jpg"
        img-alt="Image"
        img-top
        tag="article"
        style="max-width: 20rem;"
        class="mb-2"
      >
        <b-card-text>
          Rellena todos los campos necesarios para ingresar
          un vehiculo al sistema (todos los campos son obligatorios).
        </b-card-text>

        <b-form action="javascript:void(0)" @submit="onSubmit" @reset="onReset" v-if="show">
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

          <b-button
            @click="ingresarVehiculo()"
            v-if="!enEdicion"
            type="submit"
            variant="primary"
          >Aceptar</b-button>
          <b-button
            @click="actualizarVehiculo()"
            v-if="enEdicion"
            type="submit"
            variant="primary"
          >Actualizar</b-button>
          <b-button @click="onReset()" type="reset" variant="danger">Limpiar</b-button>
        </b-form>
      </b-card>
    </div>

    <div class="container2">
      <center>
        <h1>Listado de vehiculos</h1>
        <h5>Presiona recargar para actualizar la lista de vehiculos</h5>
        <b-button @click="recargarVehiculos()" variant="primary">Recargar vehiculos</b-button>
      </center>
      <br />

      <div class="container2row">
        <b-table striped hover :items="this.vehiculos" style="width:100%">
          <template v-slot:cell(acciones)="row">
            <!-- Botones para editar y eliminar aplicaciones de la lista -->
            <b-button size="sm" @click="cargarVehiculo(row)" class="mr-2">Modificar</b-button>
            <b-button
              size="sm"
              @click="eliminarVehiculo(row)"
              class="mr-2"
              variant="danger"
            >Eliminar</b-button>
            <b-button size="sm" @click="calcularPago(row)" class="mr-2" variant="success">Dar Salida</b-button>
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
      enEdicion: false,
      fecha: new Date(),
      hora: new Date().toLocaleTimeString(),
      car: 8000,
      m: 4000,
      b: 2000,
      cam: 21000,

      vehiculo: {
        tipo: null,
        placa: "",
        color: "",
        marca: "",
        cPlaca: "",
        fecha: "",
        hora: "",
        acciones: true
      },

      vehiculos: [
        {
          tipo: "Moto",
          placa: "VUP09D",
          color: "Negro",
          marca: "AKT",
          cPlaca: "Medellin",
          fecha: "1/2/2020",
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
    recargarVehiculos() {
      var personJSONFromLS = localStorage.getItem("vehiculostorage");
      var personFromLS = JSON.parse(personJSONFromLS);

      this.vehiculos = personFromLS;
    },

    ingresarVehiculo() {
      var jsonPerson = JSON.stringify(this.vehiculos);
      localStorage.setItem("vehiculostorage", jsonPerson);

      var placaBusqueda = document.getElementById("placa").value;

      if (this.vehiculos.find(vehiculo => vehiculo.placa == placaBusqueda)) {
        alert("La placa ya esta registrada");
      } else {
        this.vehiculo = {
          tipo: document.getElementById("tipoVehiculo").value,
          placa: document.getElementById("placa").value,
          color: document.getElementById("color").value,
          marca: document.getElementById("marca").value,
          cPlaca: document.getElementById("cPlaca").value,
          fecha: this.fecha,
          hora: this.hora,
          acciones: true
        };
        this.vehiculos.push(this.vehiculo);
        /*Convierte el array aplicaciones en un JSON y lo almacena en el localStorage*/
        var jsonPerson = JSON.stringify(this.vehiculos);
        localStorage.setItem("vehiculostorage", jsonPerson);
      }
    },

    eliminarVehiculo({ item }) {
      let posicion = this.vehiculos.findIndex(
        vehiculo => vehiculo.id == item.id
      );
      this.vehiculos.splice(posicion, 1);

      var jsonPerson = JSON.stringify(this.vehiculos);
      localStorage.setItem("vehiculostorage", jsonPerson);
      var personJSONFromLS = localStorage.getItem("vehiculostorage");
      var personFromLS = JSON.parse(personJSONFromLS);

      this.vehiculos = personFromLS;
    },

    cargarVehiculo({ item }) {
      let vehi = this.vehiculos.find(vehiculo => vehiculo.placa == item.placa);
      document.getElementById("tipoVehiculo").value = vehi.tipo;
      document.getElementById("placa").value = vehi.placa;
      document.getElementById("color").value = vehi.color;
      document.getElementById("marca").value = vehi.marca;
      document.getElementById("cPlaca").value = vehi.cPlaca;
      /*Cambia el estado de edicion, para hacer visible el boton Actualizar*/
      this.enEdicion = true;
    },

    actualizarVehiculo() {
      let posicion = this.vehiculos.findIndex(
        vehiculo => vehiculo.placa == this.vehiculo.placa
      );
      this.vehiculo = {
        tipo: document.getElementById("tipoVehiculo").value,
        placa: document.getElementById("placa").value,
        color: document.getElementById("color").value,
        marca: document.getElementById("marca").value,
        cPlaca: document.getElementById("cPlaca").value,
        fecha: this.fecha,
        hora: this.hora,
        acciones: true
      };
      this.vehiculos.splice(posicion, 1, this.vehiculo);

      /*Convierte el array aplicaciones en un JSON y lo almacena en el localStorage*/
      var jsonPerson = JSON.stringify(this.vehiculos);
      localStorage.setItem("vehiculostorage", jsonPerson);

      this.enEdicion = false;
    },

    calcularPago({ item }) {
      let vehi = this.vehiculos.find(vehiculo => vehiculo.placa == item.placa);
      var diaEnMils = 1000 * 60 * 60 * 24,
        desde = vehi.fecha,
        hasta = new Date(),
        diff = hasta.getTime() - desde.getTime() + diaEnMils; // +1 incluir el dia de ini
      //return diff / diaEnMils;
      let valor = 0;
      if (vehi.tipo == "Carro") {
        valor = 8000;
      } else if (vehi.tipo == "Moto") {
        valor = 4000;
      } else if (vehi.tipo == "Bicicleta") {
        valor = 2000;
      } else if (vehi.tipo == "Camion") {
        valor = 21000;
      }

      alert(
        JSON.stringify(
          "Valor a pagar: " + parseInt(diff / diaEnMils) * parseInt(valor)
        ) +
          "\n" +
          "Horas: " +
          parseInt(diff / diaEnMils)
      );
      console.log(diff / diaEnMils);
    },

    onSubmit(evt) {
      evt.preventDefault();
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