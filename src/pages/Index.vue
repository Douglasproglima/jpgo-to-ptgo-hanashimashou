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
      <div class="col-6 q-pt-md">
        <q-btn
          push color="primary"
          round size="lg" icon="keyboard_voice"
          class="float-right"
          @click="record()"/>
      </div>
      <div class="col-6 q-pt-md">
        <q-btn
        push color="primary"
        round size="lg" icon="play_arrow"
        @click="playAudio()"/>
      </div>
      <div class="col-12 text-center">
        <q-toggle
        v-model="continuous"
        label="Contínuo"
        left-label
      />
      </div>
      <div class="col-12 q-pa-xl">
        <q-input
          v-model="text"
          autogrow
          label="単語-Text"
          clearable
          outlined/>
      </div>
        <div class="col-12 q-pa-lg text-caption">
          <div class="text-bold">Instructions:</div>
          <div>Choose your language for the wizard to spell your speech correctly.</div>
          <div>Press the microphone button<q-btn dense color="primary" round size="xs" icon="keyboard_voice" />
            To start speech capture, and authorize your device to use the microphone.
          </div>
          <div>When the screen with the message "Waiting for Audio" appears, say the phrase you want to be transcribed.<br>
            When finished, your speech will appear in the Text field.
          </div>
          <div>If you want to hear the text, just hit the play button <q-btn dense color="primary" round size="xs" icon="play_arrow" /></div>
        </div>
      </div>
      <q-page-sticky v-if="btnStop" position="bottom-right" :offset="[15, 18]" style="z-index: 10000">
        <q-btn fab icon="stop" color="negative" @click="stop()" />
      </q-page-sticky>
  </q-page>
</template>

<style>

</style>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      text: '',
      voiceSelect: 'ja-JP',
      optionsVoice: [],
      continuous: false,
      btnStop: false
    }
  },
  mounted () {
    this.setVoices()
  },
  methods: {
    setVoices () {
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
    playAudio () {
      this.$speechTalk(this.voiceSelect, this.text)
    },
    record () {
      this.btnStop = true
      this.$speechToText.start(this.voiceSelect, this.continuous)
        .then((suc) => {
          this.text += ' ' + suc
          if (this.continuous) {
            this.record()
          }
          // this.btnStop = false
        })
        .catch(() => {
          this.btnStop = false
        })
    },
    stop () {
      this.$speechToText.stop()
      this.btnStop = false
    }
  }
}
</script>
