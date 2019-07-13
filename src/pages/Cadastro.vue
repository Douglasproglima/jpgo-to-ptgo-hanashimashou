<template>
  <q-page class="container">
    <div class="row q-col-gutter-md q-pt-md">
      <q-select
        outlined v-model="voiceSelect"
        :options="optionsVoice"
        label="言語 - Languages"
        class="col-12"
        emit-value
        map-options/>
       <!-- <div class="col-6 q-pt-md">
       <q-btn
          push color="primary"
          round size="lg" icon="keyboard_voice"
          class="float-right"
          @click="record()"/>
      </div> -->
        <div class="col-12 q-pt-md text-center">
          <q-btn
          push color="primary"
           size="lg" icon="play_arrow" label="Start Registration"
          @click="iniciarCadastroAudio()"/>
        </div>
    </div>
    <div class="row q-pt-md q-px-md q-col-gutter-md">
      <q-input outlined v-model="form.nome" label="名前 - Name" class="col-12"
        ref="nome"/>
      <q-input outlined v-model="form.idade" label="年齢 - Age" class="col-12"
        ref="idade" type="tek"/>
    </div>
  </q-page>
</template>

<style>
</style>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      form: {
        nome: '',
        idade: ''
      },
      voiceSelect: 'ja-JP',
      optionsVoice: []
    }
  },
  mounted () {
    this.setVoices()
  },
  methods: {
    setVoices () {
      console.log(this.optionsVoice.length)
      let id = setInterval(() => {
        if (this.optionsVoice.length === 0) {
          this.voicesList()
        } else {
          clearInterval(id)
        }
      }, 50)
    },
    voicesList () {
      let teste = window.speechSynthesis
      this.optionsVoice = teste.getVoices().map(voice => ({
        label: voice.name, value: voice.lang
      }))
    },
    // playAudio () {
    //   this.$speechTalk(this.voiceSelect, this.text)
    // },
    record (passo, callback = null) {
      this.$speechToText()
        .then((suc) => {
          passo(suc)
          if (callback) {
            callback()
          }
        })
    },
    iniciarCadastroAudio () {
      this.$speechTalk(this.voiceSelect, 'こんにちは、私たちはあなたの登録を始めます。Hello, we will start your registration.')
        .then(() => {
          this.segundoPasso()
        })
    },
    segundoPasso () {
      this.$speechTalk(this.voiceSelect, 'あなたの名前は何ですか　Whats your name ?.')
        .then(() => {
          this.record(this.setFormNome, this.terceiroPasso)
        })
    },
    terceiroPasso () {
      this.$speechTalk(this.voiceSelect, '何歳ですか How old are you ?.')
        .then(() => {
          this.record(this.setFormIdade, this.finalizar)
        })
    },
    finalizar () {
      this.$speechTalk(this.voiceSelect, '登録を完了していただきありがとうございます。 Thank you for completing your registration !')
    },
    setFormNome (nome) {
      this.form.nome = nome
      this.$refs.nome.focus()
    },
    setFormIdade (nome) {
      this.form.idade = nome
      this.$refs.idade.focus()
    }
  }
}
</script>
