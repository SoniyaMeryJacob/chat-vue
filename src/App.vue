<template>
  <div class="empty-page">
    <CollapsibleChat />
  </div>
</template>

<script>
import CollapsibleChat from './components/CollapsibleChat.vue'; // Import the collapsible chat component

export default {
  components: {
    CollapsibleChat,
  },
};
</script>

<style>
.empty-page {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f0f0f0; /* Optional background for empty page */
}
</style>
