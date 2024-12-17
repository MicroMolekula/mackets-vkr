<template>
  <div class="flex flex-col h-screen">
    <!-- Header -->
    <header class="bg-green-600 text-white p-4 flex justify-between items-center">
      <div class="text-2xl font-semibold">{{ lessonTopic }}</div>
      <div class="text-lg">{{ fullName }}</div>
    </header>

    <!-- Main Content -->
    <div class="flex flex-1">
      <!-- Subtitles Section -->
      <div class="flex-1 flex items-center justify-center p-6">
        <div class="bg-gray-800 text-white text-xl p-6 rounded-lg shadow-md w-full max-w-3xl">
          <p v-if="subtitles" class="whitespace-pre-wrap">{{ subtitles }}</p>
          <p v-else class="text-center text-gray-400">{{ subtitles }}</p>
        </div>
      </div>

      <!-- Chat Section -->
      <div class="w-96 bg-gray-100 p-4 flex flex-col">
        <div class="flex-1 overflow-y-auto mb-4">
          <div v-for="(message, index) in chatMessages" :key="index" class="mb-4">
            <div class="flex flex-col bg-white p-3 rounded-md shadow-md mb-2">
              <div class="text-sm font-semibold text-gray-700">{{ message.user }}</div>
              <div class="text-gray-600">{{ message.text.en }}</div>
              <div class="text-gray-600 text-sm">{{ message.text.ru }}</div>
            </div>
          </div>
        </div>

        <!-- Chat Input -->
        <div class="flex">
          <input
              v-model="newMessage"
              type="text"
              class="w-full p-2 border border-gray-300 rounded-l-md"
              placeholder="Введите сообщение..."
          />
          <button
              @click="sendMessage"
              class="p-2 bg-green-600 text-white rounded-r-md"
          >
            Отправить
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      lessonTopic: "Введение в нейронные сети", // Тема занятия
      fullName: "Иван Иванов", // Имя пользователя
      subtitles: "welcome to today's lesson on introduction to neural networks. neneural networks is ...",
      chatMessages: [
        {
          user: "Jone Doe",
          text: {
            en: "That sounds interesting! I'm excited to learn more.",
            ru: "Звучит интересно! Я с нетерпением жду, чтобы узнать больше."
          }
        },
        {
          user: "Kim Jong-un",
          text: {
            en: "Great! Let's get started.",
            ru: "Отлично! Давайте начнем."
          }
        }
      ],
      newMessage: "" // Для ввода нового сообщения в чат
    };
  },
  methods: {
    sendMessage() {
      if (this.newMessage.trim()) {
        const newChatMessage = {
          user: "Student", // Имя отправителя (например, Студент)
          text: {
            en: this.newMessage.trim(),
            ru: this.translateMessage(this.newMessage.trim()) // Функция для перевода на русский
          }
        };
        this.chatMessages.push(newChatMessage);
        this.newMessage = ""; // Очистить поле ввода после отправки
      }
    },
    // Простая функция для имитации перевода (в реальном проекте тут будет API для перевода)
    translateMessage(text) {
      // Пример замены английского текста на русский (на практике тут должен быть API для перевода)
      const translations = {
        "Hello": "Привет",
        "How are you?": "Как дела?",
        "Let's begin": "Давайте начнем",
        "Excited to learn": "С нетерпением жду, чтобы узнать",
        "Great!": "Отлично!"
      };
      return translations[text] || text; // Если нет перевода, возвращаем оригинал
    }
  }
};
</script>

<style scoped>
/* Основные стили для страницы */
.bg-gray-800 {
  background-color: #2d2d2d;
}
.bg-gray-100 {
  background-color: #f7fafc;
}
.text-gray-600 {
  color: #4a5568;
}
.text-gray-400 {
  color: #cbd5e0;
}
.text-green-600 {
  color: #38a169;
}
.bg-green-600 {
  background-color: #38a169;
}
.bg-green-700 {
  background-color: #2f855a;
}
</style>
