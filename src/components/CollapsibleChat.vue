<template>
  <div class="chat-wrapper">
    <!-- Chat Buttons -->
    <div class="chat-tabs">
      <div
        class="chat-tab"
        :class="{ 'above-chat': visibleChat === 'chat2' || visibleChat === 'chat3' }"
        @click="toggleChat('chat1')"
      >
        <span>CHAT 1</span>
      </div>
      <div
        class="chat-tab"
        :class="{ 'above-chat': visibleChat === 'chat3' }"
        @click="toggleChat('chat2')"
      >
        <span>CHAT 2</span>
      </div>
      <div class="chat-tab" @click="toggleChat('chat3')">
        <span>CHAT 3</span>
      </div>
    </div>

    <!-- Chat Panels -->
    <transition name="slide">
      <div v-if="visibleChat === 'chat1'" class="chat-panel">
        <ChatInterface />
      </div>
    </transition>
    <transition name="slide">
      <div v-if="visibleChat === 'chat2'" class="chat-panel">
        <ChatInterface />
      </div>
    </transition>
    <transition name="slide">
      <div v-if="visibleChat === 'chat3'" class="chat-panel">
        <ChatInterface />
      </div>
    </transition>
  </div>
</template>

<script>
import ChatInterface from './ChatInterface.vue'; // Import your chat interface component

export default {
  components: {
    ChatInterface,
  },
  data() {
    return {
      visibleChat: null, // Tracks the currently visible chat
    };
  },
  methods: {
    toggleChat(chatId) {
      // Toggle visibility of a specific chat
      this.visibleChat = this.visibleChat === chatId ? null : chatId;
    },
  },
};
</script>

<style>
/* Chat Wrapper */
.chat-wrapper {
  position: fixed;
  bottom: 0;
  right: 0;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  padding: 10px;
  z-index: 1000;
}

/* Chat Tabs Container */
.chat-tabs {
  display: flex;
  flex-direction: column;
  gap: 10px; /* Spacing between tabs */
}

/* Individual Chat Button */
.chat-tab {
  background-color: #f25c05; /* Bright orange-red */
  color: white;
  border-radius: 25px;
  padding: 10px 20px;
  font-size: 14px;
  text-align: center;
  cursor: pointer;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.chat-tab:hover {
  background-color: #d94e05; /* Darker orange for hover effect */
  transform: translateY(-2px);
}

/* Above Chat Adjustment */
.chat-tab.above-chat {
  margin-bottom: 520px; /* Adjust to align with open chat */
  z-index: 10; /* Ensure proper layering */
}

/* Chat Panel */
.chat-panel {
  width: 350px;
  height: 500px;
  background-color: #ffffff;
  border: 1px solid #ddd;
  border-radius: 10px 10px 0 0;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
  overflow: hidden;
  position: absolute;
  right: 0;
  bottom: 70px; /* Align with chat tabs */
}

/* Slide Transition */
.slide-enter-active,
.slide-leave-active {
  transition: transform 0.3s ease-in-out;
}

.slide-enter-from,
.slide-leave-to {
  transform: translateX(100%);
}

/* Responsive Design for Tablets and Phones */
@media screen and (max-width: 768px) {
  .chat-panel {
    width: 90%; /* Use 90% width on smaller screens */
    height: 50vh; /* Adjust height */
    bottom: 90px; /* Leave space for chat tabs */
  }

  .chat-tab {
    font-size: 12px; /* Smaller text size */
    padding: 8px 15px;
  }
}

@media screen and (max-width: 480px) {
  .chat-panel {
    width: 100%; /* Full width for very small screens */
    height: 50vh;
    bottom: 100px;
  }

  .chat-tab {
    font-size: 10px;
    padding: 5px 10px;
  }
}
</style>
