<template>
  <div>
    <div class="form-check" v-if="history.length > 0">
      <label class="form-check-label">
        <input class="form-check-input" type="checkbox" value @click="hist" />
        Show History?
      </label>
    </div>
    <div class="card border-primary" v-if="showHistory">
      <div class="card-header">Search History</div>
      <ul class="list-group text-left">
        <li
          class="list-group-item"
          v-for="(item, index) in paginatedData"
          :key="index"
        >
          <div>
            <strong>{{ item.fromLang }} => {{ item.toLang }}</strong>
          </div>
          <br />
          <div>
            <strong class="text-success">{{ item.fromText }} : &nbsp;</strong>
            <span class="text-info">{{ item.toText }}</span>
          </div>
        </li>
      </ul>
      <div class="card-footer">
        <div>
          <ul class="pagination">
            <li
              class="page-item text-primary"
              :class="{ disabled: pageNumber == 0 }"
            >
              <a class="page-link" @click="prevPage">&laquo;</a>
            </li>
            <li
              class="page-item text-primary"
              v-for="p in pageCount"
              :key="p"
              :class="{ active: p == pageNumber + 1 ? true : false }"
            >
              <a class="page-link" @click="setPage(p)">{{ p }}</a>
            </li>
            <li
              class="page-item text-primary"
              :class="{ disabled: pageNumber >= pageCount - 1 }"
            >
              <a class="page-link" @click="nextPage">&raquo;</a>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { value, computed } from 'vue-function-api'

export default {
  name: 'History',
  props: ['history'],
  setup(props, ctx) {
    const pageNumber = value(0)
    const size = value(4)
    const showHistory = value(false)
    const pageCount = computed(() => {
      let l = props.history.length
      let s = size.value
      return Math.ceil(l / s)
    })
    const paginatedData = computed(() => {
      let start = pageNumber.value * size.value
      let end = start + size.value
      return props.history
        .slice()
        .reverse()
        .slice(start, end)
    })
    const hist = () => {
      showHistory.value = !showHistory.value
    }
    const nextPage = () => {
      pageNumber.value++
    }
    const prevPage = () => {
      pageNumber.value--
    }
    const setPage = p => {
      pageNumber.value = p - 1
    }
    return {
      pageNumber,
      size,
      showHistory,
      pageCount,
      paginatedData,
      hist,
      nextPage,
      prevPage,
      setPage,
    }
  },
}
</script>

<style>
.pagination {
  display: flex;
  justify-content: center;
}
</style>
