<template>
  <div class="chat-container">
    <!-- Chat Header -->
    <header class="chat-header">
      <h2>Chat Interface</h2>
    </header>

    <!-- Chat Messages Display -->
    <div class="chat-messages">
      <!-- Loop through the messages -->
      <div
        v-for="(message, index) in messages"
        :key="index"
        :class="`message ${message.type}`"
      >
        <!-- Display the text content of the message -->
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

    <!-- Chat Footer (Message Input + File Upload) -->
    <footer>
      <input
        type="text"
        v-model="newMessage"
        placeholder="Type your message..."
        @keyup.enter="sendMessage"
      />
      <button class="button" @click="sendMessage">Send</button>

      <!-- File attach functionality -->
      <input
        type="file"
        ref="fileInput"
        style="display: none"
        @change="attachFile"
      />
      <button class="button attach-button" @click="triggerFileInput">
        Attach File
      </button>
    </footer>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      messages: [], // List of messages
      newMessage: "", // Current text input
    };
  },
  methods: {
    // Method to send text message
    sendMessage() {
      if (this.newMessage.trim()) {
        this.messages.push({ type: "user", content: this.newMessage });
        this.newMessage = ""; // Clear the input field after sending
      }
    },

    // Method to trigger the hidden file input element
    triggerFileInput() {
      this.$refs.fileInput.click();
    },

    // Method to handle file attachment
    async attachFile(event) {
      const file = event.target.files[0];
      if (file) {
        const formData = new FormData();
        formData.append("file", file);

        try {
          // Upload the file to the server
          const response = await axios.post("http://localhost:3001/upload", formData, {
            headers: { "Content-Type": "multipart/form-data" },
          });

          // Push the uploaded file message to the chat
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

<style scoped>
/* Basic chat container styling */
.chat-container {
  display: flex;
  flex-direction: column;
  height: 100%;
  max-width: 350px;
  background-color: #f2f2f2;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  overflow: hidden;
  position: relative;
}

/* Header styling */
.chat-header {
  background-color: #3e5159; /* Header background */
  color: white;
  padding: 15px;
  text-align: center;
  font-size: 18px;
  font-weight: bold;
}

/* Styling for messages */
.chat-messages {
  padding: 15px;
  overflow-y: auto;
  flex-grow: 1;
  max-height: 400px;
}

/* Message styles */
.message {
  margin: 10px 0;
  padding: 10px;
  border-radius: 10px;
}

.message.user {
  background-color: #f27405; /* User message background */
  color: white;
  text-align: right;
}

.message.bot {
  background-color: #3e5159; /* Bot message background */
  color: white;
  text-align: left;
}

/* File link styles */
.file-link {
  color: #1e90ff;
  text-decoration: none;
}

.file-link:hover {
  text-decoration: underline;
}

/* Footer (input area) styling */
footer {
  padding: 10px;
  background-color: #3e5159;
  display: flex;
  align-items: center;
}

footer input[type="text"] {
  flex-grow: 1;
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #ccc;
}

footer button {
  margin-left: 10px;
  padding: 10px 15px;
  background-color: #f27405;
  border: none;
  color: white;
  border-radius: 5px;
  cursor: pointer;
}

footer button.attach-button {
  background-color: #3e5159;
}

/* Hover effect for buttons */
footer button:hover {
  background-color: #f25c05;
}

footer button.attach-button:hover {
  background-color: #1e90ff;
}
</style>
