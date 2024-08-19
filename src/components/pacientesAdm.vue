<template>
  <div>
    <h2>Lista de Pacientes</h2>
    <input v-model="filtro" placeholder="Filtrar pacientes" />

    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Nombres</th>
          <th>Apellidos</th>
          <th>Cédula</th>
          <th>Genero</th>
          <th>Fecha de Nacimiento</th>
          <th>Lugar de Nacimiento</th>
          <th>Email</th>
          <th>Direccion</th>
          <th>Telefono</th>
          <th>Enfermedad</th>
          <th>Medicamentos</th>
          <th>Alergias</th>
          <th>Fecha de Creacion</th>
          <th>Activo</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(paciente, index) in pacientesFiltrados" :key="index">
          <td>{{ paciente.id }}</td>
          <td>{{ paciente.nombres }}</td>
          <td>{{ paciente.apellidos }}</td>
          <td>{{ paciente.ci }}</td>
          <td>{{ paciente.genero }}</td>
          <td>{{ paciente.fecha_nacimiento }}</td>
          <td>{{ paciente.lugar_nacimiento }}</td>
          <td>{{ paciente.email }}</td>
          <td>{{ paciente.direccion }}</td>
          <td>{{ paciente.telefono }}</td>
          <td>{{ paciente.enfermedad }}</td>
          <td>{{ paciente.medicamentos }}</td>
          <td>{{ paciente.alergias }}</td>
          <td>{{ paciente.fecha_creacion }}</td>
          <td>{{ paciente.activo ? 'Sí' : 'No' }}</td>
          <td>
            <button @click="editarPaciente(paciente)">Editar</button>
            <button @click="eliminarPaciente(paciente.id)">Eliminar</button>
          </td>
        </tr>
      </tbody>
    </table>

    <h2>Agregar/Editar Paciente</h2>
    <form @submit.prevent="guardarPaciente">
      <input v-model="nuevoPaciente.nombres" placeholder="Nombres" required />
      <input v-model="nuevoPaciente.apellidos" placeholder="Apellidos" required />
      <input v-model="nuevoPaciente.ci" placeholder="CI" required />
      <input v-model="nuevoPaciente.genero" placeholder="Genero" required />
      <input v-model="nuevoPaciente.fecha_nacimiento" type="date" placeholder="Fecha de Nacimiento" required />
      <input v-model="nuevoPaciente.lugar_nacimiento" placeholder="Lugar de Nacimiento" required />
      <input v-model="nuevoPaciente.email" type="email" placeholder="Email" required />
      <input v-model="nuevoPaciente.direccion" placeholder="Direccion" required />
      <input v-model="nuevoPaciente.telefono" placeholder="Telefono" required />
      <input v-model="nuevoPaciente.enfermedad" placeholder="Enfermedad" />
      <input v-model="nuevoPaciente.medicamentos" placeholder="Medicamentos" />
      <input v-model="nuevoPaciente.alergias" placeholder="Alergias" />
      <input v-model="nuevoPaciente.activo" type="checkbox" /> Activo
      <button type="submit">Guardar</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      filtro: '',
      pacientes: [],
      nuevoPaciente: this.inicializarPaciente()
    };
  },
  computed: {
    pacientesFiltrados() {
      return this.pacientes.filter(paciente => {
        const filtro = this.filtro.toLowerCase();
        return (
          paciente.nombres.toLowerCase().includes(filtro) ||
          paciente.apellidos.toLowerCase().includes(filtro) ||
          paciente.ci.toLowerCase().includes(filtro)
        );
      });
    }
  },
  methods: {
    inicializarPaciente() {
      return {
        id: null,
        nombres: '',
        apellidos: '',
        ci: '',
        genero: '',
        fecha_nacimiento: '',
        lugar_nacimiento: '',
        email: '',
        direccion: '',
        telefono: '',
        enfermedad: '',
        medicamentos: '',
        alergias: '',
        fecha_creacion: new Date().toISOString().split('T')[0],
        activo: true
      };
    },
    cargarPacientes() {

      fetch('http://localhost:3000/pacientes')
        .then(response => response.json())
        .then(data => {
          this.pacientes = data;
        });
    },
    guardarPaciente() {
 
      const method = this.nuevoPaciente.id ? 'PUT' : 'POST';
      const url = this.nuevoPaciente.id
        ? `http://localhost:3000/pacientes/${this.nuevoPaciente.id}`
        : 'http://localhost:3000/pacientes';

      fetch(url, {
        method,
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.nuevoPaciente)
      }).then(() => {
        this.cargarPacientes();
        this.nuevoPaciente = this.inicializarPaciente();
      });
    },
    editarPaciente(paciente) {
      this.nuevoPaciente = { ...paciente };
    },
    eliminarPaciente(id) {
      fetch(`http://localhost:3000/pacientes/${id}`, {
        method: 'DELETE'
      }).then(() => {
        this.cargarPacientes();
      });
    }
  },
  created() {
    this.cargarPacientes();
  }
};
</script>

<style scoped>

</style>
