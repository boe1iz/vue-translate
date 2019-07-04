<template>
  <div>
    <div class="card border-primary mb-3">
      <div class="card-body">
        <div class="form-group">
          <p class="card-text">
            <label for="exampleSelect1">Text to be translated:</label>
            <input type="text" class="form-control mb-3" v-model="searchText" />
            <label for="exampleSelect1">Which language to translate:</label>
            <select
              class="form-control mb-3"
              id="exampleSelect1"
              v-model="translateTo"
            >
              <option
                v-for="(lang, key) in languages"
                :value="key"
                :key="key"
                >{{ lang }}</option
              >
            </select>
            <button
              type="submit"
              class="btn btn-info btn-block"
              @click="clickTranslate"
            >
              Translate
            </button>
          </p>
          <p></p>
        </div>
      </div>
    </div>
    <div class="card border-primary mb-3" v-if="result !== ''">
      <div class="card-header">Result</div>
      <div class="card-body">
        <div class="card-text text-center text-danger">
          <label>{{ result }}</label>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { value, onCreated } from 'vue-function-api'
import axios from 'axios'

export default {
  name: 'SearchForm',
  setup(props, ctx) {
    const searchText = value('')
    const translateTo = value('')
    const languages = value({})
    const result = value('')

    onCreated(() => {
      axios
        .get(
          'https://translate.yandex.net/api/v1.5/tr.json/getLangs?key=trnsl.1.1.20181116T200129Z.775558096056e445.a0cb98993d244e1a93e7aa435b7a0f60a6b5ab4d&ui=tr'
        )
        .then(response => {
          languages.value = response.data.langs
        })
    })

    const clickTranslate = () => {
      if (searchText.value === '' || translateTo.value === '') {
        alert('Form is not fullfilled properly!')
        return
      }
      let url =
        'https://translate.yandex.net/api/v1.5/tr.json/translate?key=trnsl.1.1.20181116T200129Z.775558096056e445.a0cb98993d244e1a93e7aa435b7a0f60a6b5ab4d&text=' +
        searchText.value +
        '&lang=' +
        translateTo.value
      axios
        .get(url)
        .then(response => {
          let pack = {
            fromLang: languages.value[response.data.lang.split('-')[0]],
            toLang: languages.value[translateTo.value],
            fromText: searchText.value,
            toText: response.data.text[0],
          }
          searchText.value = ''
          result.value = pack.toText
          ctx.emit('emitTranslatePack', pack)
          //   ctx.root.$emit("emitTranslatePack", pack);
          //   this.$emit("emitTranslatePack", pack);
        })
        .catch(err => {
          console.log(err)
        })
    }

    return {
      searchText,
      translateTo,
      languages,
      result,
      clickTranslate,
    }
  },
}
</script>
