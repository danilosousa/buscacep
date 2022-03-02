<template>
  <div id="app">
    <h1>busca ceps</h1>
    <div class="form-group">
      <label for="">CEP:</label>
      <input
        type="text"
        @keyup="getCeps"
        v-model="zipCode"
        placeholder="Digite seu cep"
      />
    </div>
    <div class="form-group">
      <label for="">Logadouro</label>
      <input type="text" placeholder="Logadouro" v-model="logadouro" />
    </div>
    <div class="form-group">
      <label for="">Complemento</label>
      <input type="text" placeholder="complemento" v-model="complemento" />
    </div>
    <div class="form-group">
      <label for="">Bairro</label>
      <input type="text" placeholder="bairro" v-model="bairro" />
    </div>
    <div class="form-group">
      <label for="">Localidade</label>
      <input type="text" placeholder="Localidade" v-model="localidade" />
    </div>
    <div class="form-group">
      <label for="">UF</label>
      <input type="text" v-model="uf" placeholder="uf" />
    </div>

    <div class="" v-if="addressResult">resultado === {{ addressResult }}</div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      zipCode: "",
      logadouro: "",
      bairro: "",
      complemento: "",
      localidade: "",
      uf: "",
      addressResult: null,
    };
  },
  methods: {
    getCeps() {
      const baseURI = `http://viacep.com.br/ws/${this.zipCode}/json/`;
      if (this.zipCode.length > 7) {
        const zip = this.zipCode.toString().replace("-", "");
        if (!isNaN(zip) && zip.length === 8) {
          this.$http
            .get(baseURI)
            .then((result) => {
              this.addressResult = result.data;
              console.log(this.addressResult);
              this.zipCode = this.addressResult.cep;
              this.logadouro = this.addressResult.logadouro;
              this.bairro = this.addressResult.bairro;
              this.complemento = this.addressResult.complemento;
              this.localidade = this.addressResult.localidade;
              this.uf = this.addressResult.uf;
            })
            .catch((error) => {
              console.log(error);
              return error;
            });
        }
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
