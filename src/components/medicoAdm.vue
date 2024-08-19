<template>
  <div>
    <h2>Médicos</h2>
    <form @submit.prevent="guardarMedico">
      <input v-model="medico.nombres" placeholder="Nombres" />
      <input v-model="medico.apellidos" placeholder="Apellidos" />
      <input v-model="medico.ci" placeholder="CI" />
      <input v-model="medico.genero" placeholder="Genero" />
      <input v-model="medico.fechaNacimiento" type="date" placeholder="Fecha de Nacimiento" />
      <input v-model="medico.lugarNacimiento" placeholder="Lugar de Nacimiento" />
      <input v-model="medico.email" type="email" placeholder="Email" />
      <input v-model="medico.direccion" placeholder="Direccion" />
      <input v-model="medico.telefono" type="tel" placeholder="Telefono" />
      <input v-model="medico.especialidad" placeholder="Especialidad" />
      <button type="submit">Guardar</button>
    </form>

    <input v-model="filtro" placeholder="Buscar por nombre o CI" @input="filtrarMedicos" />
    <ul>
      <li v-for="med in medicosFiltrados" :key="med.id">
        {{ med.nombres }} {{ med.apellidos }} - {{ med.ci }}
        <button @click="editarMedico(med.id)">Editar</button>
        <button @click="eliminarMedico(med.id)">Eliminar</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      medico: {
        id: null,
        nombres: '',
        apellidos: '',
        ci: '',
        genero: '',
        fechaNacimiento: '',
        lugarNacimiento: '',
        email: '',
        direccion: '',
        telefono: '',
        especialidad: '',
        fechaCreacion: '',
        activo: true
      },
      medicos: [],
      filtro: '',
    };
  },
  computed: {
    medicosFiltrados() {
      return this.medicos.filter(med =>
        med.nombres.toLowerCase().includes(this.filtro.toLowerCase()) ||
        med.ci.toLowerCase().includes(this.filtro.toLowerCase())
      );
    }
  },
  methods: {
    guardarMedico() {
      if (this.medico.id === null) {
        this.medico.id = Date.now();
        this.medico.fechaCreacion = new Date().toISOString();
        this.medicos.push({ ...this.medico });
      } else {
        const index = this.medicos.findIndex(m => m.id === this.medico.id);
        this.medicos[index] = { ...this.medico };
      }
      this.resetForm();
    },
    editarMedico(id) {
      const medico = this.medicos.find(m => m.id === id);
      this.medico = { ...medico };
    },
    eliminarMedico(id) {
      this.medicos = this.medicos.filter(m => m.id !== id);
    },
    filtrarMedicos() {

    },
    resetForm() {
      this.medico = {
        id: null,
        nombres: '',
        apellidos: '',
        ci: '',
        genero: '',
        fechaNacimiento: '',
        lugarNacimiento: '',
        email: '',
        direccion: '',
        telefono: '',
        especialidad: '',
        fechaCreacion: '',
        activo: true
      };
    }
  }
};
</script>

<style scoped>

</style>
