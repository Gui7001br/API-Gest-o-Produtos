<template>
  <header>
    <v-container>
      <v-sheet width="500" class="mx-auto">
        <h1> Cadastrar Produto </h1>
        <v-form fast-fail @submit.prevent>
          <v-text-field v-model="produto.nome" label="Nome do Produto"></v-text-field>

          <v-text-field v-model="produto.preco" label="Preço do Produto" type="number"></v-text-field>

          <v-btn type="submit" color="primary" block class="mt-2" @click="cadastrarProduto(novoProduto)">Cadastrar</v-btn>
        </v-form>
      </v-sheet>
    </v-container>
    <hr>
    <v-container>
      <v-sheet width="500" class="mx-auto">
        <h1> Listagem de Produtos </h1>
        <p v-if="variaveis.erroAPI"> Sem produtos para mostrar</p>

        <v-table v-if="!variaveis.erroAPI">
          <thead>
            <tr>
              <th class="text-left"> Nome </th>
              <th class="text-left"> Preço </th>
              <th class="text-left"> Ações </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="produto in variaveis.produtos" :key="produto.name">
              <td>{{ produto.nome }}</td>
              <td>{{ produto.preco }}</td>
              <td>
                <v-btn class="ma-2" color="error" @click="removerProduto(produto)">
                  <v-icon start icon="mdi-delete"></v-icon>
                  Remover
                </v-btn>
              </td>
            </tr>
          </tbody>
        </v-table>
        <v-btn color="primary" @click="listarProdutos">Listar Produtos</v-btn>
      </v-sheet>
    </v-container>
  </header>
</template>

<script setup>
import { reactive, toRaw } from 'vue';
import axios from 'axios';

const produto = reactive({
  nome: '',
  preco: ''
})
const variaveis = reactive({
  erroAPI: true,
  produtos: []
})

function listarProdutos() {
  axios.get('http://localhost:3000/produtos')
    .then(response => {
      variaveis.produtos = response.data;
      variaveis.erroAPI = false;
    })
    .catch(function () {
      variaveis.erroAPI = true
    });
    
  /* console.log('fazer GET no endpoint /produtos da API');
  console.log('Se der certo o GET atualizar:');
  console.log('- variaveis.produtos recebendo a resposta da API');
  console.log('- variaveis.erroAPI recebendo false se a requisição deu certo');
  console.log('- variaveis.erroAPI recebendo true se a requisição deu errado');
  console.log('Dica: vc deve fazer aqui o axios.get...');
   fazer GET no endpoint /produtos da API
   Se der certo o GET atualizar:
   variaveis.produtos recebendo a resposta da API
   variaveis.erroAPI recebendo verdadeiro se a requisição deu certo
   variaveis.erroAPI recebendo falso se a requisição deu errado
  
  Dica: vc deve fazer aqui o axios.get...

  Para exemplo de como fazer requisição GET no axios:
  https://axios-http.com/ptbr/docs/example */
}

async function removerProduto(produtoASerRemovido) {
  produtoASerRemovido = { data: produtoASerRemovido }; //não mexer!

  const bet =  await axios.delete('http://localhost:3000/produtos',  produtoASerRemovido );
     if (bet.status === 200) {
      listarProdutos(),
      variaveis.erroAPI = false
    } else {
      variaveis.erroAPI = true
    }

  /* console.log('Remover o produto fazendo requisição DELETE no endpoint /produtos');
  console.log('O objeto produtoASerRemovido já é o que deve ser passado na requisição');
  console.log('Caso a deleção ocorra sem problemas vc deve chamar o método listarProdutos() para que a lista dos produtos seja atualizada na tela');
  console.log('Caso dê erro vc deve setar a variaveis.erroAPI = true');
  console.log('Dica: vc deve fazer aqui o axios.delete...');
  console.log(produtoASerRemovido);
  Remover o produto fazendo requisição DELETE no endpoint /produtos
  O objeto produtoASerRemovido já é o que deve ser passado na requisição
  Caso a deleção ocorra sem problemas vc deve chamar o método listarProdutos() para que a lista dos produtos seja atualizada na tela
  Caso dê erro vc deve setar a variaveis.erroAPI = true
  
  Dica: vc deve fazer aqui o axios.delete...
  Para exemplo de como fazer requisição DELETE no axios:
  https://masteringjs.io/tutorials/axios/delete-with-body */
}

function cadastrarProduto() {
  const novoProduto = produto; //não mexer

  axios.post('http://localhost:3000/produtos', novoProduto)
    .then(() => {
      produto.nome = ''; //não mexer
      produto.preco = ''; //não mexer
      listarProdutos();
    })
    .catch(error => {
      console.log(error)
    })

  /* console.log('fazer POST no endpoint /produtos da API');
  console.log('passando como parâmetro a constante novoProduto');
  console.log('Dica: vc deve fazer aqui o axios.post...');
  console.log(novoProduto);
  fazer POST no endpoint /produtos da API
  passando como parâmetro a constante novoProduto

  Dica: vc deve fazer aqui o axios.post...
  Para exemplo de como fazer requisição POST no axios:
  https://axios-http.com/ptbr/docs/post_example

  Ao cadastrar um produto vc pode "limpar" os atributos nome e preco
  utilizando o exemplo de código abaixo */

}
</script>