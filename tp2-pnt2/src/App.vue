<template>
  <div class="container-fluid mt-3">
    <input
      type="text"
      class="form-control"
      v-model="criterioDeBusquedaNombre"
      placeholder="Buscar por nombre o apellido"
    />
    <br />
    <input
      type="text"
      class="form-control"
      v-model="criterioDeBusquedaDni"
      placeholder="Buscar por DNI"
    />
    <br />
    <div v-if="mostrarAdvertencia" class="alert alert-warning" role="alert">
      Ingresá al menos 3 caracteres en alguno de los filtros
    </div>
    <div class="card-deck m-0">
      <div class="row">
        <div class="col" v-for="persona in personasFiltradas" :key="persona.dni">
          <div class="card mb-3">
            <div class="card-body">
              <h5 class="card-title">{{ getNombreCompleto(persona) }}</h5>
              <p class="card-text">dni {{ persona.dni }}</p>
              <a href="#" class="card-link">{{ persona.correo }}</a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      criterioDeBusquedaNombre: '',
      criterioDeBusquedaDni: '',
      personas: [
        {
          nombre: 'Daniel',
          apellido: 'Sanchez',
          correo: 'danielsanchez68@hotmail.com',
          dni: '20442873',
        },
        {
          nombre: 'Juan',
          apellido: 'Perez',
          correo: 'j@p.gmail.com',
          dni: '12345678',
        },
        {
          nombre: 'Ana',
          apellido: 'Suarez',
          correo: 'a@s.gmail.com',
          dni: '87654321',
        },
        {
          nombre: '...',
          apellido: '...',
          correo: '...',
          dni: '...',
        },
      ],
    }
  },
  computed: {
    mostrarAdvertencia() {
      return (
        (this.criterioDeBusquedaNombre.length > 0 &&
          this.criterioDeBusquedaNombre.length < 3) ||
        (this.criterioDeBusquedaDni.length > 0 && this.criterioDeBusquedaDni.length < 3)
      )
    },
    personasFiltradas() {
      return this.personas.filter((persona) => {
        if (
          this.criterioDeBusquedaNombre === '' &&
          this.criterioDeBusquedaDni === ''
        ) {
          return true
        }
        const nombreCompleto = `${persona.nombre} ${persona.apellido}`.toLowerCase()
        const filtroNombre = this.criterioDeBusquedaNombre.toLowerCase()
        const dni = persona.dni.toLowerCase()
        const filtroDni = this.criterioDeBusquedaDni.toLowerCase()

        if (
          filtroNombre.length >= 3 &&
          filtroDni.length >= 3
        ) {
          return (
            nombreCompleto.includes(filtroNombre) && dni.includes(filtroDni)
          )
        } else if (filtroNombre.length >= 3) {
          return nombreCompleto.includes(filtroNombre)
        } else if (filtroDni.length >= 3) {
          return dni.includes(filtroDni)
        } else {
          return false
        }
      })
    },
  },
  methods: {
    getNombreCompleto(persona) {
      return `${persona.nombre} ${persona.apellido}`
    },
  },
}
</script>

<style>
/* Si querés, podés agregar estilos acá */
</style>
