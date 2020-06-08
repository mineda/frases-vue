<template>
  <div>
    <form @submit.prevent="cadastrar">
      <h2>Frases Motivacionais</h2>
      <div class="form-group">
        <label for="titulo">Título</label>
        <input type="text" id="titulo"
            class="form-control" required autofocus
            v-model="titulo">
      </div>
      <div class="form-group">
        <label for="frase">Frase</label>
        <textarea id="frase"
            class="form-control" required
            v-model="frase">
        </textarea>
      </div>
      <button class="btn btn-lg btn-primary btn-block" 
        type="submit">Salvar</button>
    </form>
    <br>
    <table class="table table-striped">
      <thead>
        <tr>
          <th>Id</th>
          <th>Título</th>
          <th>Frase</th>
          <th>Data/hora</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="frase in frases" :key="frase.id">
          <td>{{ frase.id }}</td>
          <td>{{ frase.titulo }}</td>
          <td>{{ frase.conteudo }}</td>
          <td>{{ frase.dataHora }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios'
import { mapState } from 'vuex'
export default {
  name: 'anotacoes',
  data() {
    return {
      titulo: '',
      frase: '',
      frases: []
    }
  },
  computed: {
    ...mapState([
      'usuario'
    ])
  },
  methods: {
    cadastrar() {
      axios.post('frase/nova',
          {
            titulo: this.titulo,
            conteudo: this.frase,
            usuario: this.usuario
          })
        .then(res => {
          console.log(res);
          this.titulo = '';
          this.frase = '';
          this.atualizar();
        })
        .catch(error => console.log(error))
    },
    atualizar () {
      axios.get('/frase/busca/' + this.usuario, 
          { headers: { Accept: 'application/json' } })
        .then(res => {
          console.log(res)
          this.frases = res.data
        })
        .catch(error => console.log(error))
    }
  },
  created () {
    this.atualizar()
  }
}
</script>