<template>
  <div class="container-deputado-view">
    <div 
      class="content"
    >
      <div class="deputado-title">
        <div class="deputado-nome">
          {{ deputado.nomeCivil }}
        </div>
        <div class="deputado-subtitle">
          <div class="deputado-subcontent">
            <b>Partido:</b>
            {{ deputado.ultimoStatus.siglaPartido }}
          </div>
          <div class="deputado-subcontent">
            <b>UF:</b>
            {{ deputado.ultimoStatus.siglaUf }}
          </div>
          <div class="deputado-subcontent">
            <b>Situação:</b>
            {{ deputado.ultimoStatus.situacao }}
          </div>
        </div>
        <div class="deputado-sub">
          <div class="deputado-subcontent">
            <b>Escolaridade:</b>
            {{ deputado.escolaridade }}
          </div>
          <div class="deputado-subcontent">
            <b>Condição Eleitoral:</b>
            {{ deputado.ultimoStatus.condicaoEleitoral }}
          </div>
        </div>
        <div class="deputado-sub">
          <div class="deputado-subcontent">
            <b>Local de nascimento:</b>
            {{ deputado.municipioNascimento }}
            | {{ deputado.ufNascimento }}
          </div>
        </div>
        <div class="deputado-sub">
          <div class="deputado-subcontent">
            <b>Email:</b>
            {{ deputado.ultimoStatus.gabinete.email }}
          </div>
        </div>
        <div class="deputado-sub">
          <div class="deputado-subcontent">
            <b>Nascimento:</b>
            {{ deputado.dataNascimento }}
          </div>
        </div>
        <div class="deputado-sub">
          <div class="deputado-subcontent">
            <b>Número:</b>
            {{ deputado.ultimoStatus.gabinete.telefone }}
          </div>
        </div>
      </div>
      <div class="deputado-foto">
        <img :src="deputado.ultimoStatus.urlFoto" style="width: 150%;">
      </div>
    </div>


    <div
      class="box-card"
    >

      <div 
        class="container-Card"
        @click="selectDespesas()"
      >
        <p style="font-size: 90%;"><b>Despesas</b></p>
        <font-awesome-icon
            :icon="iconDespesas"
            class="font-awesome"
        />
      </div>


      <div
        class="each-card"
        :style="displayCardDespesas"
        v-for="despesas in deputadoDespesas"
        :key="despesas.numDocumento"
      >
        <div
          class="content-card"
          @click="selectCard(despesas.numDocumento)"
        >
          <p style="font-size: 90%;">{{ despesas.tipoDespesa }} <b>({{ despesas.dataDocumento }})</b></p>
          <font-awesome-icon
            v-if="despesas.numDocumento != cardSelected"
            icon="caret-right"
            class="font-awesome-despesas"
          />
          <font-awesome-icon
            v-if="despesas.numDocumento == cardSelected"
            icon="caret-down"
            class="font-awesome-despesas"
          />
        </div>
        <div
          class="card-info"
          v-if="despesas.numDocumento == cardSelected"
        > 
          <div class="deputado-sub">
            <div class="deputado-subcontent">
              <b>Nome do Fornecedor:</b>
              {{ despesas.nomeFornecedor }}
            </div>
            <div class="deputado-subcontent">
              <b>Tipo de Documento:</b>
              {{ despesas.tipoDocumento }}
            </div>
          </div>
          <div class="deputado-sub">
            <div class="deputado-subcontent">
              <b>Valor do Documento:</b>
              {{ despesas.valorDocumento }}
            </div>
            <div class="deputado-subcontent">
              <b>Valor Liquido:</b>
              {{ despesas.valorLiquido }}
            </div>
          </div>
          <div class="deputado-sub">
            <div class="deputado-subcontent">
              <b>Documento:</b>
              {{ despesas.urlDocumento }}
            </div>
          </div>
        </div>
      </div>


      <div 
        class="container-Card"
        @click="selectOrgao()"
      >
        <p style="font-size: 90%;"><b>Orgãos</b></p>
        <font-awesome-icon
            :icon="iconOrgao"
            class="font-awesome"
        />
      </div>

      <div
        class="each-card"
        :style="displayCardOrgao"
        v-for="orgao in deputadoOrgao"
        :key="orgao.__ob__.dep.idOrgao"
      >
        <div
          class="content-card"
          @click="selectCard(orgao.idOrgao)"
        >
          <p style="font-size: 90%;"><b>{{ orgao.siglaOrgao }}</b></p>
          <font-awesome-icon
            v-if="orgao.idOrgao != cardSelected"
            icon="caret-right"
            class="font-awesome-despesas"
          />
          <font-awesome-icon
            v-if="orgao.idOrgao == cardSelected"
            icon="caret-down"
            class="font-awesome-despesas"
          />
        </div>
        <div
          class="card-info"
          v-if="orgao.idOrgao == cardSelected"
        > 
          <div class="deputado-sub">
            <div class="deputado-subcontent">
              <b>Titulo:</b>
              {{ orgao.titulo }}
            </div>
          </div>
          <div class="deputado-sub">
            <div class="deputado-subcontent">
              <b>Nome do Orgão:</b>
              {{ orgao.nomeOrgao }}
            </div>
          </div>
          <div class="deputado-sub">
            <div class="deputado-subcontent">
              <b>Informações:</b>
              {{ orgao.uriOrgao }}
            </div>
          </div>
        </div>
      </div>

      <div 
        class="container-Card"
        @click="selectEventos()"
      >
        <p style="font-size: 90%;"><b>Eventos</b></p>
        <font-awesome-icon
            :icon="iconEvento"
            class="font-awesome"
        />
      </div>

      <div
        class="each-card"
        :style="displayCardEvento"
        v-for="evento in deputadoEventos"
        :key="evento.id"
      >
        <div
          class="content-card"
          @click="selectCard(evento.id)"
        >
          <p style="font-size: 90%;"><b>{{ evento.descricaoTipo }}</b></p>
          <font-awesome-icon
            v-if="evento.id != cardSelected"
            icon="caret-right"
            class="font-awesome-despesas"
          />
          <font-awesome-icon
            v-if="evento.id == cardSelected"
            icon="caret-down"
            class="font-awesome-despesas"
          />
        </div>
        <div
          class="card-info"
          v-if="evento.id == cardSelected"
        > 
          <div class="deputado-sub">
            <div class="deputado-subcontent">
              <b>Situação:</b>
              {{ evento.situacao }}
            </div>
          </div>
          <div class="deputado-sub">
            <div class="deputado-subcontent">
              <b>Orgãos:</b>
              <div
                v-for="orgao in evento.orgaos"
                :key="orgao.__ob__.dep.id"
              >
              - {{ orgao.nome }} ({{ orgao.sigla }})
              </div>
            </div>
          </div>
          <div class="deputado-sub">
            <div class="deputado-subcontent">
              <b>Descrição:</b>
              {{ evento.descricao }}
            </div>
          </div>
          <div class="deputado-sub">
            <div 
              v-if=" evento.localCamara == null"
              class="deputado-subcontent"
            >
              <b>Local:</b>
              {{ evento.localExterno.nome }}
            </div>
            <div 
              class="deputado-subcontent"
              v-else-if=" evento.localExterno == null"
            >
              <b>Local:</b>
              {{ evento.localCamara.nome }}
            </div>
            <div 
              class="deputado-subcontent"
              v-else
            >
              <b>Local: --- </b>
            </div>
          </div>
        </div>
      </div>


      <div 
        class="container-Card"
        @click="selectDiscurso()"
      >
        <p style="font-size: 90%;"><b>Discursos</b></p>
        <font-awesome-icon
            :icon="iconDiscurso"
            class="font-awesome"
        />
      </div>

      <div
        class="each-card"
        :style="displayCardDiscurso"
        v-for="discurso in deputadoDiscurso"
        :key="discurso.__ob__.dep.id"
      >
        <div
          class="content-card"
          @click="selectCard(discurso.id)"
        >
          <p style="font-size: 90%;"><b>{{ discurso.tipoDiscurso }}</b></p>
          <font-awesome-icon
            v-if="discurso.id != cardSelected"
            icon="caret-right"
            class="font-awesome-despesas"
          />
          <font-awesome-icon
            v-if="discurso.id == cardSelected"
            icon="caret-down"
            class="font-awesome-despesas"
          />
        </div>
        <div
          class="card-info"
          v-if="discurso.id == cardSelected"
        > 
          <div class="deputado-sub">
            <div 
              v-if="discurso.faseEvento.titulo != null"
              class="deputado-subcontent"
            > 
              <b>Evento:</b>
              {{ discurso.faseEvento.titulo }}
            </div>
          </div>
          <div class="deputado-sub">
            <div 
              v-if="discurso.transcricao != null"
              class="deputado-subcontent"
            > 
              <b>Transcrição:</b>
              {{ discurso.transcricao }}
            </div>
          </div>
          <div class="deputado-sub">
            <div 
              v-if="discurso.urlAudio != null"
              class="deputado-subcontent"
            > 
              <b>Audio:</b>
              {{ discurso.urlAudio }}
            </div>
          </div>
          <div class="deputado-sub">
            <div 
              v-if="discurso.urlTexto != null"
              class="deputado-subcontent"
            > 
              <b>Texto:</b>
              {{ discurso.urlTexto }}
            </div>
          </div>
          <div class="deputado-sub">
            <div 
              v-if="discurso.urlVideo != null"
              class="deputado-subcontent"
            > 
              <b>Video:</b>
              {{ discurso.urlVideo }}
            </div>
          </div>
        </div>
      </div>    


      <div 
        class="container-Card"
        @click="selectFrente()"
      >
        <p style="font-size: 90%;"><b>Frentes</b></p>
        <font-awesome-icon
            :icon="iconFrente"
            class="font-awesome"
        />
      </div>

      <div 
        class="card"
        :style="displayCardFrente"
      >
        <p>Frentes que perticipa:</p>
        <div
          class="each-card"
          v-for="frente in deputadoFrente"
          :key="frente.id"
        >
          <div
            class="cardInfo"
          > 
            - {{ frente.titulo }}
          </div>
        </div>    
      </div>
    </div>

  </div>
</template>





<script>
import axios from 'axios'

export default {
  data() {
    return {
      deputado: [],
      deputadoDespesas: [],
      deputadoOrgao: [],
      deputadoEventos: [],
      deputadoDiscurso: [],
      deputadoFrente: [],
      displayCardDespesas: 'display: none;',
      displayCardOrgao: 'display: none;',
      displayCardEvento: 'display: none;',
      displayCardDiscurso: 'display: none;',
      displayCardFrente: 'display: none;',
      iconDespesas: 'caret-right',
      iconFrente: 'caret-right',
      iconDiscurso: 'caret-right',
      iconOrgao: 'caret-right',
      iconEvento: 'caret-right',
      cardSelected: '',
    }
  },
  mounted() {
    this.request();
  },
  methods:{
    request(){
      axios
        .get("https://dadosabertos.camara.leg.br/api/v2/deputados/74646/")
        .then(response => {
          this.deputado = response.data.dados
        })
        .catch(error => {
          console.log(error.response)
        })
      axios
        .get("https://dadosabertos.camara.leg.br/api/v2/deputados/74646/despesas")
        .then(response => {
          this.deputadoDespesas = response.data.dados
        })
        .catch(error => {
          console.log(error.response)
        })
      axios
        .get("https://dadosabertos.camara.leg.br/api/v2/deputados/74646/orgaos")
        .then(response => {
          this.deputadoOrgao = response.data.dados
        })
        .catch(error => {
          console.log(error.response)
        })
      axios
        .get("https://dadosabertos.camara.leg.br/api/v2/deputados/74646/eventos")
        .then(response => {
          this.deputadoEventos = response.data.dados
        })
        .catch(error => {
          console.log(error.response)
        })
      axios
        .get("https://dadosabertos.camara.leg.br/api/v2/deputados/74646/discursos")
        .then(response => {
          this.deputadoDiscurso = response.data.dados
        })
        .catch(error => {
          console.log(error.response)
        })
      axios
        .get("https://dadosabertos.camara.leg.br/api/v2/deputados/74646/frentes")
        .then(response => {
          this.deputadoFrente = response.data.dados
        })
        .catch(error => {
          console.log(error.response)
        })
    },
    selectDespesas(){
      if(this.iconDespesas == 'caret-right'){
        this.iconDespesas = 'caret-down'
        this.displayCardDespesas = 'display: flex;'
      }
      else if(this.iconDespesas == 'caret-down'){        
        this.iconDespesas = 'caret-right'
        this.displayCardDespesas = 'display: none;'
      }
    },
    selectOrgao(){
      if(this.iconOrgao == 'caret-right'){
        this.iconOrgao = 'caret-down'
        this.displayCardOrgao = 'display: flex;'
      }
      else if(this.iconOrgao == 'caret-down'){        
        this.iconOrgao = 'caret-right'
        this.displayCardOrgao = 'display: none;'
      }
    },
    selectEventos(){
      if(this.iconEvento == 'caret-right'){
        this.iconEvento = 'caret-down'
        this.displayCardEvento = 'display: flex;'
      }
      else if(this.iconEvento == 'caret-down'){        
        this.iconEvento = 'caret-right'
        this.displayCardEvento = 'display: none;'
      }
    },
    selectDiscurso(){
      if(this.iconDiscurso == 'caret-right'){
        this.iconDiscurso = 'caret-down'
        this.displayCardDiscurso = 'display: flex;'
      }
      else if(this.iconDiscurso == 'caret-down'){        
        this.iconDiscurso = 'caret-right'
        this.displayCardDiscurso = 'display: none;'
      }
    },
    selectFrente(){
      if(this.iconFrente == 'caret-right'){
        this.iconFrente = 'caret-down'
        this.displayCardFrente = 'display: flex;'
      }
      else if(this.iconFrente == 'caret-down'){        
        this.iconFrente = 'caret-right'
        this.displayCardFrente = 'display: none;'
      }
    },
    selectCard(value){
      if(this.cardSelected == ''){
        this.cardSelected = value
      }
      else if (this.cardSelected != '' && this.cardSelected != value){
        this.cardSelected = value
      }
      else {
        this.cardSelected = ''
      }
    }
  }
}
</script>





<style>

.content {
  display: flex;
  justify-content: space-between;
}

.container-deputado-view {
  margin: 5%; 
  width: 60%;
  height: 100%;
}

.deputado-nome {
  display: flex;
  font-size: 30px;
  font-weight: bold;
  color: #0C5E3D;
}

.deputado-subtitle{
  display: flex;
  font-size: 80%;
  margin-top: 15%;
  justify-content: space-between;
}

.deputado-sub{
  display: flex;
  text-align: initial;
  font-size: 80%;
  margin-top: 2%;
  justify-content: space-between;
}

.deputado-foto {
  margin-right: 10%;
}

.container-Card{
  padding: 0px 15px;
  margin: 10px 0px 5px 0px;
  text-align: left;
  font-size: 80%;
  background-color: #2f7958;
  border-radius: 10px;
  color: white;
  display: flex;
  justify-content: space-between;
}

.content-card{
  padding: 0px 15px;
  margin: 10px 0px 5px 0px;
  text-align: left;
  font-size: 80%;
  background-color: #f8f8f8;
  border-radius: 10px;
  display: flex;
  justify-content: space-between;
  font-size: 70%;
}

.card-info{
  margin: 0px 20px 25px 20px;
}

.cardInfo{
  text-align: initial;
  padding: 3px 10px;
}

.each-card{
  display: flex;
  flex-direction: column; 
}

.box-card{
  margin-top: 8%;
}

.card{
  display: flex;
  flex-direction: column; 
  padding: 15px;
  margin: 10px 0px 5px 0px;
  text-align: left;
  background-color: #f8f8f8;
  border-radius: 10px;
  font-size: 70%;
}

.font-awesome {
  margin: 9.5px 0px;
  font-size: 130%;
  color: white;
}

.font-awesome-despesas {
  margin: 9.5px 0px;
  font-size: 130%;
  color: #1a1919b6;
}

</style>