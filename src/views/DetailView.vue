<template>
  <div>
    <loader v-if="loading"/>
    <div v-else-if="record">
      <div class="breadcrumb-wrap">
        <router-link to="/history" class="breadcrumb">История</router-link>
        <a @click.prevent class="breadcrumb">
          {{ record.typeText }}
        </a>
      </div>
      <div class="row">
        <div class="col s12 m6">
          <div class="card" :class="record.typeClass">
            <div class="card-content white-text">
              <p>Описание: {{ record.description }}</p>
              <p>Сумма: {{ $filters.currencyFilter(record.amount) }}</p>
              <p>Категория: {{ record.categoryName }}</p>

              <small>{{ $filters.dateFilter(record.date, 'datetime')}}</small>
            </div>
          </div>
        </div>
      </div>
    </div>
    <p class="center" v-else>Запись с id={{$route.params.id}} не найдена</p>
  </div>
</template>


<script>
import {useMeta} from "vue-meta";

export default {
  name: "DetailRecordView",
  components: {},
  setup() {
    useMeta({ title: 'Запись'})
  },
  data: () => ({
    loading: true,
    record: null,
    category: null,
  }),
  async mounted() {
    const id = this.$route.params.id
    const record = await this.$store.dispatch('fetchRecordById', id)
    const category = await this.$store.dispatch('fetchCategoryById', record.categoryId)

    this.record = {
      ...record,
      categoryName: category.title,
      typeClass: record.type === 'income' ? 'green' : 'red',
      typeText: record.type === 'income' ? 'Доход' : 'Расход'
    }

    this.loading = false
  }
}
</script>

<style scoped>

</style>