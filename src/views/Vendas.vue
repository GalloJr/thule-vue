<template>
  <div>
    <form @submit.prevent="cadastrar">
      <h2>Vendas</h2>
      <div class="form-group">
        <label for="cliente">Cliente</label>
        <input type="text" id="cliente"
            class="form-control" required autofocus
            v-model="cliente"> 
            <!-- era titulo virou cliente -->
      </div>
      <div class="form-group">
        <label for="grp">Categoria</label>
        <textarea id="grp"
            class="form-control" required
            v-model="grp">
            <!-- era frase virou grp -->
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
          <th>Cliente</th>
          <th>Categoria</th>
          <th>Data/hora</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="venda in vendas" :key="venda.id">
          <td>{{ venda.id }}</td>
          <td>{{ venda.cliente }}</td>
          <td>{{ venda.grp }}</td>
          <td>{{ venda.dataHora }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios'
import { mapState } from 'vuex'
export default {
  name: 'vendas', //era anotacoes
  data() {
    return {
      cliente: '',
      venda: '',
      vendas: []
    }
  },
  computed: {
    ...mapState([
      'usuario'
    ])
  },
  methods: {
    cadastrar() {
      axios.post('venda/nova',
          {
            cliente: this.cliente,
            venda: this.venda,
            usuario: this.usuario
          })
        .then(res => {
          console.log(res);
          this.cliente = '';
          this.grp = '';
          this.atualizar();
        })
        .catch(error => console.log(error))
    },
    atualizar () {
      axios.get('/venda/busca/' + this.usuario, 
          { headers: { Accept: 'application/json' } })
        .then(res => {
          console.log(res)
          this.vendas = res.data
        })
        .catch(error => console.log(error))
    }
  },
  created () {
    this.atualizar()
  }
}
</script>