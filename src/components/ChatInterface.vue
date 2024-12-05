<template>
  <div class="chat-container">
    <header class="chat-header">Chat Interface</header>
    <div class="chat-messages">
      <!-- Loop through messages -->
      <div
        v-for="(message, index) in messages"
        :key="index"
        :class="`message ${message.type}`"
      >
        <!-- Display text messages -->
        <span v-if="message.content">{{ message.content }}</span>

        <!-- Display file link if the message contains a file -->
        <a
          v-if="message.fileUrl"
          :href="message.fileUrl"
          target="_blank"
          class="file-link"
        >
          {{ message.fileName }}
        </a>
      </div>
    </div>
    <footer>
      <input
        type="text"
        v-model="newMessage"
        placeholder="Type your message..."
        @keyup.enter="sendMessage"
      />
      <button class="button" @click="sendMessage">Send</button>

      <!-- File attach functionality -->
      <input type="file" ref="fileInput" style="display: none" @change="attachFile" />
      <button class="button attach-button" @click="triggerFileInput">Attach File</button>
    </footer>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      messages: [],
      newMessage: "",
    };
  },
  methods: {
    sendMessage() {
      if (this.newMessage.trim()) {
        this.messages.push({ type: "user", content: this.newMessage });
        this.newMessage = "";
      }
    },
    triggerFileInput() {
      this.$refs.fileInput.click();
    },
    async attachFile(event) {
      const file = event.target.files[0];
      if (file) {
        const formData = new FormData();
        formData.append("file", file);

        try {
          const response = await axios.post("http://localhost:3001/upload", formData, {
            headers: { "Content-Type": "multipart/form-data" },
          });
          const fileUrl = `http://localhost:3001/${response.data.filename}`;
          this.messages.push({
            type: "user",
            fileUrl,
            fileName: response.data.originalname,
          });
        } catch (error) {
          console.error("Error uploading file:", error);
        }
      }
    },
  },
};
</script>

<style src="../assets/theme.css"></style>
