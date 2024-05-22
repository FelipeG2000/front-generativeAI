<template>
  <div class="container">
    <h1>Sistema de preguntas y respuestas con IA generativa</h1>
    <div class="input-group">
      <input type="text" v-model="pregunta" class="form-control" placeholder="Ingrese su pregunta" @keyup.enter="enviarPregunta">
      <button @click="enviarPregunta" class="btn btn-primary">Enviar</button>
    </div>

    <div v-if="respuesta" class="mt-3">
      <div class="pregunta-box">
        <b>Pregunta:</b>
        <p>{{ respuesta.question }}</p>
      </div>
      <div class="respuesta-box">
        <b>Respuesta:</b>
        <p>{{ respuesta.respuesta }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      pregunta: '',
      respuesta: null,
    };
  },
  methods: {
    enviarPregunta() {
      if (this.pregunta.trim()) {
        axios.post('http://127.0.0.1:8000/api/answer/', { question: this.pregunta })
          .then(response => {
            this.respuesta = response.data;
          })
          .catch(error => {
            console.error(error);
          })
          .finally(() => {
            this.pregunta = '';
          });
      }
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.input-group {
  display: flex;
  align-items: center;
}

.form-control {
  flex: 1;
  padding: 5px;
  border: 1px solid #ccc;
}

.btn {
  padding: 5px 10px;
  border: none;
  cursor: pointer;
}

.pregunta-box,
.respuesta-box {
  border: 1px solid #ccc;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 5px;
}

.pregunta-box {
  background-color: #f5f5f5;
}

.respuesta-box {
  background-color: #e0ffe0;
}
</style>