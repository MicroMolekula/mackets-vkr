<template>
  <div class="flex">
    <!-- Sidebar -->
    <aside class="w-64 bg-green-600 text-white h-screen p-6">
      <div class="text-2xl font-semibold mb-8">LiveTranslateEdu</div>

      <!-- User info in Sidebar -->
      <div class="mb-8">
        <div class="text-lg font-medium">{{ fullName }}</div>
      </div>

      <!-- Navigation links in Sidebar -->
      <nav class="space-y-6">
        <ul>
          <li>
            <a
                href="#"
                @click="currentPage = 'lessons'"
                class="text-white hover:text-green-200 text-lg font-semibold"
            >
              Доступные занятия
            </a>
          </li>
          <li>
            <a
                href="#"
                @click="currentPage = 'history'"
                class="text-white hover:text-green-200 text-lg font-semibold"
            >
              История занятий
            </a>
          </li>
          <li>
            <a
                href="#"
                @click="currentPage = 'create'"
                class="text-white hover:text-green-200 text-lg font-semibold"
            >
              Создать занятие
            </a>
          </li>
        </ul>
      </nav>
    </aside>

    <!-- Main Content -->
    <main class="flex-1 bg-gray-50 p-6">
      <!-- Dynamic Content for Lesson History -->
      <div v-if="currentPage === 'history'">
        <h2 class="text-2xl font-semibold text-green-600 mb-4">История занятий</h2>

        <!-- If no lessons have been joined -->
        <div v-if="history.length === 0">
          <p>Вы еще не присоединились к занятиям.</p>
        </div>

        <!-- List of joined lessons -->
        <ul class="space-y-6">
          <li v-for="(lessonItem, index) in history" :key="index" class="bg-white p-6 rounded-lg shadow-lg hover:shadow-xl transition-shadow duration-300">
            <h3 class="text-xl font-semibold text-gray-800">{{ lessonItem.topic }}</h3>
            <p class="text-gray-600 mt-2 mb-4">{{ lessonItem.description }}</p>

            <!-- Display additional details -->
            <div class="text-sm text-gray-600">
              <p><strong>Аудитория:</strong> {{ lessonItem.audience }}</p>
              <p><strong>Преподаватель:</strong> {{ lessonItem.teacher }}</p>
              <p><strong>Присоединились:</strong> {{ formatDate(lessonItem.joinDate) }}</p>
            </div>
          </li>
        </ul>
      </div>

      <!-- Other Pages -->
      <div v-if="currentPage === 'lessons'">
        <h2 class="text-2xl font-semibold text-green-600 mb-4">Доступные занятия</h2>
        <p>Здесь будет отображаться список доступных занятий.</p>
      </div>

      <div v-if="currentPage === 'create'">
        <h2 class="text-2xl font-semibold text-green-600 mb-4">Создать занятие</h2>
        <p>Форма для создания нового занятия.</p>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentPage: 'history', // Переключение между страницами
      user: {
        firstName: "Иван",
        lastName: "Иванов",
      },
      // История занятий
      history: [
        {
          topic: "Введение в нейронные сети",
          description: "",
          audience: "255",
          teacher: "Александров Алексей Викторович",
          startTime: "2024-11-25T10:00:00", // ISO формат
          joinDate: "2024-11-25T10:05:00", // Дата, когда пользователь присоединился
        },
        {
          topic: "Экспертные системы",
          description: "",
          audience: "Л1",
          teacher: "Сергеев Иван Павлович",
          startTime: "2024-12-02T10:00:00", // ISO формат
          joinDate: "2024-12-02T10:01:00", // Дата, когда пользователь присоединился
        },
      ],
    };
  },
  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`;
    },
  },
  methods: {
    // Форматирование даты
    formatDate(dateString) {
      const date = new Date(dateString);
      return date.toLocaleString(); // Форматирует дату и время в локальный формат
    },
  },
};
</script>

<style scoped>
/* Дополнительные стили для боковой панели и контента */
</style>
