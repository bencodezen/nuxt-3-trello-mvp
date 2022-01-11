<script setup>
import { computed, ref } from 'vue'

const props = defineProps({
  data: Object,
  migrateList: Array,
  parentColumn: String
})

const emits = defineEmits({
  'migrate-item': null
})

const migrateListTarget = ref('')

const filteredMigrateList = computed(() => {
  return props.migrateList.filter(
    item => item.columnName !== props.parentColumn
  )
})

const migrateCard = () => {
  const targetColumn = migrateListTarget.value
  const originalColumn = props.parentColumn

  if (targetColumn !== originalColumn) {
    emits('migrate-item', {
      cardId: props.data.id,
      targetColumn,
      originalColumn
    })
  }
}
</script>

<template>
  <section class="base-card">
    <h4 style="margin-top: 0">{{ data.name }}</h4>
    <select
      name="luffy-migrate-list"
      id="luffy-migrate-list"
      style="padding: 5px; width: 100%; margin-bottom: 10px"
      v-model="migrateListTarget"
    >
      <option
        v-for="column in filteredMigrateList"
        :key="`migrate-${column.columnName}`"
        :value="column.columnName"
      >
        {{ column.columnName }}
      </option>
    </select>
    <button @click="migrateCard">Migrate</button>
  </section>
</template>

<style scoped>
.base-card {
  border: 1px solid #222;
  padding: 10px;
  background-color: #fff;
}
</style>
