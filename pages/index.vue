<script setup>
import { ref } from 'vue'
import { workspaceList } from '../store/global.js'

const newWorkspaceName = ref('')
const createWorkspace = () => {
  const randomId = Math.floor(Math.random() * 100)

  workspaceList.value.push({
    id: randomId,
    name: newWorkspaceName.value
  })

  newWorkspaceName.value = ''
}
</script>

<template>
  <main class="main-content">
    <h1>Home Page</h1>
    <h2>Workspaces</h2>
    <input
      type="text"
      v-model="newWorkspaceName"
      @keyup.enter="createWorkspace"
      style="width: 100%; padding: 5px; font-size: 1rem; margin-bottom: 10px"
    />
    <button @click="createWorkspace" style="padding: 7px 8px 5px">
      Create a Workspace
    </button>
    <ul class="workspace-list">
      <li
        v-for="workspace in workspaceList"
        :key="workspace.id"
        class="workspace-card"
      >
        <nuxt-link :to="`/workspace/${workspace.id}`"
          >{{ workspace.id }}: {{ workspace.name }}</nuxt-link
        >
      </li>
    </ul>
  </main>
</template>

<style>
.main-content {
  padding: 30px;
}

.main-content h1 {
  margin-top: 0;
}

.workspace-card {
  display: block;
  border: 2px solid #222;
  border-radius: 4px;
  padding: 2rem;
  margin-bottom: 1rem;
}

.workspace-list {
  margin-left: 0;
  padding-left: 0;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-column-gap: 10px;
}
</style>
