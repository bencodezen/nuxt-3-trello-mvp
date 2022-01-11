<script setup>
defineProps({
  data: Object,
  migrateList: Array,
  parentColumn: String
})
</script>

<template>
  <section class="base-card">
    <h4 style="margin-top: 0">{{ data.name }}</h4>
    <select
      name="luffy-migrate-list"
      id="luffy-migrate-list"
      style="padding: 5px; width: 100%; margin-bottom: 10px"
    >
      <option
        v-for="column in migrateList"
        :key="`migrate-${column.columnName}`"
        :value="column.columnName"
      >
        {{ column.columnName }}
      </option>
    </select>
    <button>Migrate</button>
  </section>
</template>

<style scoped>
.base-card {
  border: 1px solid #222;
  padding: 10px;
  background-color: #fff;
}
</style>
