<script setup>
import { computed, ref, watchEffect } from 'vue'

const props = defineProps({
  data: Object,
  migrateList: Array,
  parentColumn: String,
  position: Object
})

const emits = defineEmits({
  'migrate-item': null
})

const migrateListTarget = ref('')
const initialX = ref(0)

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

onMounted(() => {
  initialX.value = props.position.x
})

watchEffect(() => {
  if (props.position.x - initialX.value > 250) {
    console.log('hello')
    emits('migrate-item', {
      cardId: props.data.id,
      targetColumn: 'Zorro',
      originalColumn: props.parentColumn
    })
  }
})
</script>

<template>
  <section class="base-card">
    <h4 style="margin-top: 0">{{ data.name }}</h4>
    <p>Position: {{ position }}</p>
    <p>Original Position: {{ initialX }}</p>
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
