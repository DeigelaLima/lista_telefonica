<script setup lang="ts">

   import { ref,onMounted } from 'vue';

   const mensagem = ref('Lista Telefônica');
   const contatos = ref({
        id: 0,
        nome: null,
        telefone: null,
        endereco: null,
   });
   const index = ref<number | null>(null);
   const lista = ref<any[]>([]);


    //Carregando os dados e configurando na propriedade lista
    onMounted(() => { 
       const contatosData = JSON.parse(localStorage.getItem('contatos'))
        lista.value = contatosData ? contatosData : []
    })
    
    const add = () => {			
         if(!contatos.value.nome || !contatos.value.telefone){
             alert("Por favor, preencha todos os campos obrigatórios :)")
             return
        }
         if (contatos.value.id === 0) {
             const maxId = Math.max(...lista.value.map(item => item.id), 0)
             contatos.value.id = maxId + 1;
             lista.value.push({...contatos.value})
         }
         else if(index.value !== null) {
            // Atualização de contato existente
           // Substitui o objeto de contato na posição Index da lista pelo contato
           lista.value[index.value] = {...contatos.value}
         }
          // Armazena a lista de contatos -> Limpa
        localStorage.setItem('contatos', JSON.stringify(lista.value))
        contatos.value = {id: 0, nome: null, telefone: null, endereco: null}
    }
  

    const remove = (item: any) => {
        // Encontra o indice do contato que será removido
        const itemIndex = lista.value.indexOf(item)
        // Remove contato na posição Idx 
        // 2º parâmentro indica que um contato será removido
        lista.value.splice(itemIndex, 1)
        // Atualiza o local mostrando a lista atualizada
        // Converte a lista atual para JSON
        // Amazena a string na chave contatos
        localStorage.setItem('contatos', JSON.stringify(lista.value))
    }
    
    const edit = (item: any) => {
        // Amazena o contato editado
        index.value = lista.value.indexOf(item)
        // Copia os dados do item para contatos
        // Cria um novo objeto vazio e copia o item para um novo objeto
        contatos.value = {...item}
        localStorage.setItem('contatos', JSON.stringify(lista.value))
    }	

</script>

<template>
  <div id="info">
      <h1 class="titulo">{{ mensagem }}</h1>
      <div class="col-2">
          <label>Nome</label>
          <input class="input-nome" v-model="contatos.nome" required> 
          <label>Telefone</label>
          <input type="text" v-model="contatos.telefone" id="telefone-input" required>
          <label>Endereço</label>
          <input type="text" v-model="contatos.endereco" id="endereco-input">

          <div class="button-add">
              <button class="mt-3 mb-3 btn btn-primary" 
        @click="add()">Adicionar</button>
          </div>
      </div>
      
      <table class="table">
          <thead>
              <tr>
                  <th scope="col">#</th>
                  <th scope="col">Nome</th>
                  <th scope="col">Telefone</th>
                  <th scope="col">Endereço</th>
                  <th scope="col">Ação</th>
              </tr>
          </thead>
          <tbody>
              <tr v-for="item in lista">
                  <th scope="row">{{ item.id }}</th>
                  <td>{{ item.nome }}</td>
                  <td>{{ item.telefone }}</td>
                  <td>{{ item.endereco }}</td>
                  <td>
                      <button @click="edit(item)" class="btn btn-info">Editar</button>
                      <button @click="remove(item)" class="btn btn-danger">Excluir</button>
                  </td>
              </tr>
          </tbody>
      </table>
  </div>
  
</template>

<style>
  /* Estilos para o contêiner principal */
  #info {
    margin: 20px auto;
    max-width: 800px;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
    background-color: #f5f5f5;
  }

  /* Estilos para o título */
  .titulo {
    background-color: #007bff;
    color: #fff;
    padding: 10px;
    text-align: center;
    margin-bottom: 20px;
  }

  /* Estilos para a área de entrada - input */
  .col-2 {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin-bottom: 20px;
  }

  /* Estilo os Nome e Telefone */
  .col-2 label {
    font-weight: bold;
  }
  .input-nome input[type="text"], .input-nome input[type="number"]{
    padding: 8px;
    border: 1px solid #0f0e0e;
    border-radius: 5px;
  }

  /* Estilos para os botão Adicionar */
  .col-2 button { 
    padding: 10px 20px;
    background-color: #007bff;
    color: #fff;
    border: none;
    width: 40%;
    border-radius: 5px;
    cursor: pointer;
  }

  .button-add {
    display: flex;
    justify-content: center; 
    align-items: center;
    width: 80%; 
    margin-left: 60px;
  }

  /* Estilo do botão Adicionar */
  .col-2 button:hover {
    background-color: #0056b3;
  }

  /* Estilos para a tabela */
  .table {
    width: 100%;
    border-collapse: collapse;
  }

  /* Configuração das bordas da tabela */
  .table th,
  .table td {
    border: 2px solid #0f0e0e;
    padding: 8px;
    text-align: center;
  }

  /* Estilos para os botões de ação na tabela */
  .table .btn {
    padding: 6px 10px;
    margin-right: 5px;
    border-radius: 5px;
    cursor: pointer;
  }

  /* Botão editar */
  .table .btn.btn-info {
    background-color: #17a2b8;
    color: #fff;
  }

  /* Botão excluir */
  .table .btn.btn-danger {
    background-color: #dc3545;
    color: #fff;
  }
</style>
