<template>
  <div>
    <h2>Citas</h2>
    <form @submit.prevent="guardarCita">
      <input v-model="cita.titulo" placeholder="Titulo" />
      <textarea v-model="cita.descripcion" placeholder="Descripcion"></textarea>
      <select v-model="cita.medicoId">
        <option v-for="medico in medicos" :key="medico.id" :value="medico.id">
          {{ medico.nombres }} {{ medico.apellidos }}
        </option>
      </select>
      <select v-model="cita.pacienteId">
        <option v-for="paciente in pacientes" :key="paciente.id" :value="paciente.id">
          {{ paciente.nombres }} {{ paciente.apellidos }}
        </option>
      </select>
      <input v-model="cita.fechaAtencion" type="date" placeholder="Fecha de Atencion" />
      <input v-model="cita.horaInicioAtencion" type="time" placeholder="Hora de Inicio" />
      <input v-model="cita.horaFinalAtencion" type="time" placeholder="Hora de Finalizacion" />
      <input v-model="cita.precio" type="number" placeholder="Precio" />
      <input v-model="cita.pago" type="number" placeholder="Pago" />
      <textarea v-model="cita.observaciones" placeholder="Observaciones"></textarea>
      <button type="submit">Guardar</button>
    </form>

    <input v-model="filtro" placeholder="Buscar por título o paciente" @input="filtrarCitas" />
    <ul>
      <li v-for="cit in citasFiltradas" :key="cit.id">
        {{ cit.titulo }} - Paciente: {{ obtenerNombrePaciente(cit.pacienteId) }} - Médico: {{ obtenerNombreMedico(cit.medicoId) }}
        <button @click="editarCita(cit.id)">Editar</button>
        <button @click="eliminarCita(cit.id)">Eliminar</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cita: {
        id: null,
        titulo: '',
        descripcion: '',
        medicoId: '',
        pacienteId: '',
        fechaAtencion: '',
        horaInicioAtencion: '',
        horaFinalAtencion: '',
        estado: '',
        observaciones: '',
        precio: '',
        pago: '',
        fechaCreacion: '',
        activo: true
      },
      citas: [],
      medicos: [],  
      pacientes: [], 
      filtro: '',
    };
  },
  computed: {
    citasFiltradas() {
      return this.citas.filter(cit =>
        cit.titulo.toLowerCase().includes(this.filtro.toLowerCase()) ||
        this.obtenerNombrePaciente(cit.pacienteId).toLowerCase().includes(this.filtro.toLowerCase())
      );
    }
  },
  methods: {
    guardarCita() {
      if (this.cita.id === null) {
        this.cita.id = Date.now();
        this.cita.fechaCreacion = new Date().toISOString();
        this.citas.push({ ...this.cita });
      } else {
        const index = this.citas.findIndex(c => c.id === this.cita.id);
        this.citas[index] = { ...this.cita };
      }
      this.resetForm();
    },
    editarCita(id) {
      const cita = this.citas.find(c => c.id === id);
      this.cita = { ...cita };
    },
    eliminarCita(id) {
      this.citas = this.citas.filter(c => c.id !== id);
    },
    filtrarCitas() {

    },
    obtenerNombrePaciente(id) {
      const paciente = this.pacientes.find(p => p.id === id);
      return paciente ? `${paciente.nombres} ${paciente.apellidos}` : 'Desconocido';
    },
    obtenerNombreMedico(id) {
      const medico = this.medicos.find(m => m.id === id);
      return medico ? `${medico.nombres} ${medico.apellidos}` : 'Desconocido';
    },
    resetForm() {
      this.cita = {
        id: null,
        titulo: '',
        descripcion: '',
        medicoId: '',
        pacienteId: '',
        fechaAtencion: '',
        horaInicioAtencion: '',
        horaFinalAtencion: '',
        estado: '',
        observaciones: '',
        precio: '',
        pago: '',
        fechaCreacion: '',
        activo: true
      };
    }
  }
};
</script>

<style scoped>

</style>
