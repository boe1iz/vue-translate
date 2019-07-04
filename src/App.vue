<template>
  <div id="app" class="container mt-2 col-md-6 col-xs-12">
    <h2 class="text-center">Translator Vue</h2>
    <div class="text-right mb-3">
      <span class="font-italic">by Bülent Öner (feat. Yandex API)</span>
    </div>
    <SearchForm @emitTranslatePack="emitTranslatePack" />
    <History :history="history" />
  </div>
</template>

<script>
import { value } from 'vue-function-api'
import SearchForm from './components/SearchForm'
import History from './components/History'

export default {
  name: 'App',
  components: {
    SearchForm,
    History,
  },
  setup() {
    const history = value([])
    const emitTranslatePack = payload => {
      history.value.push(payload)
    }
    return {
      history,
      emitTranslatePack,
    }
  },
}
</script>

<style>
@media screen and (max-width: 768px) {
  h2 {
    font-size: 24px;
  }
  span {
    font-size: 14px;
  }
}
body {
  background: #f3f2f2;
}
</style>
