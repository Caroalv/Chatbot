<template>
    <div class="flex justify-center items-center h-screen bg-gray-100">
      <div class="w-full max-w-sm h-screen bg-white shadow-lg rounded-lg flex flex-col overflow-hidden">
        <!-- Ventana de chat -->
        <div class="chat-container" ref="chatWindow">
          <div v-for="(message, index) in messages" :key="index" :class="{'text-right': message.isUser, 'text-left': !message.isUser}">
            <div v-if="message.isUser" class="bubble user-bubble">
              {{ message.text }}
            </div>
            <div v-else class="flex items-start">
              <!-- Ícono del bot -->
              <img src="@/assets/bot-avatar.png" alt="Bot Avatar" class="w-8 h-8 rounded-full mr-2" />
              <div class="bubble bot-bubble">
                <img v-if="message.image" :src="message.image" :alt="message.text" class="rounded-lg mb-2" />
                <span>{{ message.text }}</span>
              </div>
            </div>
          </div>
        </div>
  
        <!-- Área de entrada -->
        <div class="p-4 border-t border-gray-200 flex items-center bg-white">
          <textarea 
            v-model="question" 
            placeholder="Haz una pregunta" 
            rows="1"
            :class="{'overflow-auto': question.length > 50, 'overflow-hidden': question.length <= 50}"
            class="flex-1 border border-gray-300 rounded-lg px-3 py-2 resize-none focus:outline-none focus:ring-2 focus:ring-blue-500"
            @input="adjustHeight"
            @keyup.enter="sendMessage"
            ref="inputField"
          />
          <button @click="sendMessage" class="bg-blue-500 text-white rounded-full px-4 py-2 ml-2">
            Enviar
          </button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import '@/style.css'; // Importa el archivo CSS
  
  export default {
    data() {
      return {
        question: '',
        messages: [
          { text: '¡Hola! ¿En qué puedo ayudarte hoy?', isUser: false }
        ]
      };
    },
    methods: {
      async sendMessage() {
        if (this.question.trim()) {
          this.messages.push({ text: this.question, isUser: true });
  
          try {
            const response = await fetch('https://yesno.wtf/api');
            const data = await response.json();
  
            this.messages.push({ text: data.answer.charAt(0).toUpperCase() + data.answer.slice(1), image: data.image, isUser: false });
  
            this.$nextTick(() => {
              this.$refs.chatWindow.scrollTop = this.$refs.chatWindow.scrollHeight;
            });
  
          } catch (error) {
            console.error('Error al obtener la respuesta:', error);
          }
          
          this.question = '';
          this.resetHeight();
        }
      },
      adjustHeight() {
        const textarea = this.$refs.inputField;
        textarea.style.height = 'auto';
        textarea.style.height = `${textarea.scrollHeight}px`;
      },
      resetHeight() {
        const textarea = this.$refs.inputField;
        textarea.style.height = 'auto';
      }
    },
    mounted() {
      this.adjustHeight();
    }
  };
  </script>
  