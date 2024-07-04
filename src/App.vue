<script setup>
import { ref } from 'vue'

const usuario = ref({
  nome: '',
  sobrenome: '',
  email: '',
  cidade: '',
  estado: '',
  zip: '',
  avatar: '',
  hobbies: [],
  linguagemPref: ''
})

const estados = [
  { uf: 'AC', nome: 'Acre' },
  { uf: 'AL', nome: 'Alagoas' },
  { uf: 'AP', nome: 'Amapá' },
  { uf: 'AM', nome: 'Amazonas' },
  { uf: 'BA', nome: 'Bahia' },
  { uf: 'CE', nome: 'Ceará' },
  { uf: 'DF', nome: 'Distrito Federal' },
  { uf: 'ES', nome: 'Espírito Santo' },
  { uf: 'GO', nome: 'Goiás' },
  { uf: 'MA', nome: 'Maranhão' },
  { uf: 'MT', nome: 'Mato Grosso' },
  { uf: 'MS', nome: 'Mato Grosso do Sul' },
  { uf: 'MG', nome: 'Minas Gerais' },
  { uf: 'PA', nome: 'Pará' },
  { uf: 'PB', nome: 'Paraíba' },
  { uf: 'PR', nome: 'Paraná' },
  { uf: 'PE', nome: 'Pernambuco' },
  { uf: 'PI', nome: 'Piauí' },
  { uf: 'RJ', nome: 'Rio de Janeiro' },
  { uf: 'RN', nome: 'Rio Grande do Norte' },
  { uf: 'RS', nome: 'Rio Grande do Sul' },
  { uf: 'RO', nome: 'Rondônia' },
  { uf: 'RR', nome: 'Roraima' },
  { uf: 'SC', nome: 'Santa Catarina' },
  { uf: 'SP', nome: 'São Paulo' },
  { uf: 'SE', nome: 'Sergipe' },
  { uf: 'TO', nome: 'Tocantins' }
]

const mostrarPerfil = ref(false)

function handleFileUpload(e) {
  const target = e.target
  console.log(target)
  if (target && target.files) {
    const file = target.files[0]
    usuario.value.avatar = URL.createObjectURL(file)
  }
}

function salvarPerfil() {
  mostrarPerfil.value = true
}
</script>

<template>
  <div class="container">
    <main>
      <h1>Editor de Perfil</h1>
      <transition nome="form" mode="out-in">
        <section v-if="mostrarPerfil">
          <div class="mt-5 mb-3">
            <p v-for="(value, key) of usuario" :key="key">{{ key }}: {{ value }}</p>
            <img v-if="usuario.avatar" class="avatar" :src="usuario.avatar" />
          </div>
          <button class="btn btn-info" @click="mostrarPerfil = false">Esconder</button>
        </section>
        <form v-else class="row g-3 was-validated" @submit.prevent="salvarPerfil()" validate>
          <div class="col-md-4">
            <label for="nomeField" class="form-label">Nome</label>
            <input type="text" class="form-control" id="nomeField" v-model="usuario.nome" required />
            <div class="invalid-feedback">Nome obrigatório</div>
          </div>
          <div class="col-md-4">
            <label for="sobrenomeField" class="form-label">Sobrenome</label>
            <input
              type="text"
              class="form-control"
              id="sobrenomeField"
              v-model="usuario.sobrenome"
              required
            />
            <div class="invalid-feedback">Sobrenome obrigatório</div>
          </div>
          <div class="col-md-4">
            <label for="emailField" class="form-label">E-mail</label>
            <div class="input-group">
              <span class="input-group-text" id="emailFieldPrepend">@</span>
              <input
                type="email"
                class="form-control"
                id="emailField"
                aria-describedby="emailFieldPrepend"
                v-model="usuario.email"
                required
              />
              <div class="invalid-feedback">E-mail obrigatório.</div>
              <div class="valid-feedback">E-mail válido!</div>
            </div>
          </div>
          <div class="col-md-2">
            <label for="cidadeField" class="form-label">Cidade</label>
            <input type="text" class="form-control" id="cidadeField" v-model="usuario.cidade" />
          </div>
          <div class="col-md-2">
            <label for="estadoField" class="form-label">Estado</label>
            <select class="form-select" id="estadoField" v-model="usuario.estado">
              <option selected disabled value="">Selecionar...</option>
              <option v-for="estado of estados" :key="estado.uf" :value="estado.uf">
                {{ estado.nome }}
              </option>
            </select>
          </div>
          <div class="col-md-2">
            <label for="zipField" class="form-label">CEP</label>
            <input type="text" class="form-control" id="zipField" v-model="usuario.zip" />
          </div>
          <div class="col-md-6">
            <label for="avatarField" class="form-label">Avatar</label>
            <input
              type="file"
              class="form-control"
              id="avatarField"
              @change="handleFileUpload($event)"
            />
          </div>
          <div class="col-6">
            <p class="mb-0">Hobbies</p>
            <input
              class="ms-3 me-1"
              type="checkbox"
              id="hobbiesField"
              value="esportes"
              v-model="usuario.hobbies"
            />
            <label for="hobbiesField">Esportes</label>
            <input
              class="ms-3 me-1"
              type="checkbox"
              id="hobbiesField"
              value="música"
              v-model="usuario.hobbies"
            />
            <label for="hobbiesField">Música</label>
            <input
              class="ms-3 me-1"
              type="checkbox"
              id="hobbiesField"
              value="viagens"
              v-model="usuario.hobbies"
            />
            <label for="hobbiesField">Viagens</label>
            <input
              class="ms-3 me-1"
              type="checkbox"
              id="hobbiesField"
              value="leitura"
              v-model="usuario.hobbies"
            />
            <label for="hobbiesField">Leitura</label>
          </div>
          <div class="col-6">
            <p class="mb-0">Linguagem preferida</p>
            <input
              class="ms-3 me-1"
              type="radio"
              v-model="usuario.linguagemPref"
              value="C"
              id="langC"
            />
            <label for="langC">C</label>
            <input
              class="ms-3 me-1"
              type="radio"
              v-model="usuario.linguagemPref"
              value="Java"
              id="langJava"
            />
            <label for="langJava">Java</label>
            <input
              class="ms-3 me-1"
              type="radio"
              v-model="usuario.linguagemPref"
              value="Python"
              id="langPython"
            />
            <label for="langPython">Python</label>
            <input
              class="ms-3 me-1"
              type="radio"
              v-model="usuario.linguagemPref"
              value="Javascript"
              id="langJs"
            />
            <label for="langJs">JavaScript</label>
          </div>
          <div class="col-12">
            <button class="btn btn-primary" type="submit">Enviar</button>
          </div>
        </form>
      </transition>
    </main>
  </div>
</template>

<style scoped>
.avatar {
  width: 200px;
  height: 200px;
  border-radius: 50%;
}

.form-enter-active,
.form-leave-active {
  transition: opacidade 0.5s ease;
}

.form-enter-from,
.form-leave-to {
  opacidade: 0;
}
</style>