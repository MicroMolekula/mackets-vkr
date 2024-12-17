<template>
  <div class="flex flex-col h-screen">
    <!-- Header -->
    <header class="bg-green-600 text-white p-4 flex justify-between items-center">
      <div class="text-2xl font-semibold">Админ-панель</div>
      <div>
        <button
            @click="currentPage = 'users'"
            :class="currentPage === 'users' ? 'text-green-300' : 'text-white'"
            class="px-4"
        >
          Пользователи
        </button>
        <button
            @click="currentPage = 'groups'"
            :class="currentPage === 'groups' ? 'text-green-300' : 'text-white'"
            class="px-4"
        >
          Группы
        </button>
      </div>
    </header>

    <!-- Content -->
    <div class="flex-1 p-6">
      <!-- Groups Page -->
      <div v-if="currentPage === 'groups'">
        <div class="flex justify-between items-center mb-6">
          <h2 class="text-2xl font-semibold text-green-600">Управление группами</h2>
          <button
              @click="openModal('group')"
              class="bg-green-600 text-white px-4 py-2 rounded"
          >
            Добавить группу
          </button>
        </div>

        <!-- Groups Table -->
        <table class="min-w-full table-auto border-collapse border border-gray-300">
          <thead>
          <tr class="bg-gray-100">
            <th class="border border-gray-300 py-2 px-4 text-left">Название группы</th>
            <th class="border border-gray-300 py-2 px-4 text-left">Студенты</th>
            <th class="border border-gray-300 py-2 px-4 text-left">Действия</th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="(group, index) in groups" :key="index" class="border-b">
            <td class="border border-gray-300 py-2 px-4">{{ group.name }}</td>
            <td class="border border-gray-300 py-2 px-4">
              <ul>
                <li v-for="student in group.students" :key="student.id">{{ student }}</li>
              </ul>
            </td>
            <td class="border border-gray-300 py-2 px-4">
              <button
                  @click="openAddStudentModal(index)"
                  class="bg-blue-500 text-white px-4 py-2 rounded mr-2"
              >
                Добавить студента
              </button>
              <button
                  @click="deleteGroup(index)"
                  class="bg-red-500 text-white px-4 py-2 rounded"
              >
                Удалить
              </button>
            </td>
          </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- Modals -->
    <div v-if="showModal" class="fixed inset-0 bg-gray-900 bg-opacity-50 flex items-center justify-center z-50">
      <div class="bg-white p-6 rounded-lg shadow-lg max-w-md w-full">
        <h3 class="text-lg font-semibold mb-4">
          {{ modalType === 'user' ? 'Добавить пользователя' : modalType === 'group' ? 'Добавить группу' : 'Добавить студента в группу' }}
        </h3>

        <!-- Add Group Form -->
        <form v-if="modalType === 'group'" @submit.prevent="addGroup">
          <div>
            <label for="groupName" class="block text-sm font-medium text-gray-700">Название группы</label>
            <input
                v-model="newGroup.name"
                type="text"
                id="groupName"
                class="mt-1 block w-full px-4 py-2 border border-gray-300 rounded-md"
                required
            />
          </div>
          <button type="submit" class="bg-green-600 text-white px-4 py-2 rounded mt-4">
            Сохранить
          </button>
        </form>

        <!-- Add Student to Group Form -->
        <form v-if="modalType === 'addStudent'" @submit.prevent="addStudentsToGroup">
          <div>
            <label for="students" class="block text-sm font-medium text-gray-700">Студенты</label>
            <select
                v-model="selectedStudents"
                id="students"
                multiple
                class="mt-1 block w-full px-4 py-2 border border-gray-300 rounded-md"
            >
              <option
                  v-for="student in availableStudents"
                  :key="student.id"
                  :value="student.firstName + ' ' + student.lastName"
              >
                {{ student.firstName }} {{ student.lastName }}
              </option>
            </select>
          </div>
          <button type="submit" class="bg-green-600 text-white px-4 py-2 rounded mt-4">
            Добавить
          </button>
        </form>

        <button
            @click="closeModal"
            class="absolute top-2 right-2 text-gray-500 hover:text-gray-700"
        >
          ✕
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentPage: "groups", // Страница (users/groups)
      showModal: false,
      modalType: "", // Тип модального окна ('group' или 'addStudent')
      selectedGroupIndex: null, // Индекс группы для добавления студентов
      selectedStudents: [], // Выбранные студенты для добавления
      users: [
        { id: 1, firstName: "Иван", lastName: "Иванов", email: "ivan@example.com", role: "student", language: "Русский" },
        { id: 2, firstName: "Мария", lastName: "Петрова", email: "maria@example.com", role: "teacher", language: "Английский" },
        { id: 3, firstName: "Сергей", lastName: "Кузнецов", email: "sergey@example.com", role: "student", language: "Русский" },
      ],
      groups: [{ name: "PI-21-1", students: ["Иван Иванов"] }],
      newGroup: { name: "", students: [] },
    };
  },
  computed: {
    availableStudents() {
      // Возвращаем только пользователей с ролью "student"
      return this.users.filter(user => user.role === "student");
    },
  },
  methods: {
    openModal(type) {
      this.modalType = type;
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
      this.selectedStudents = [];
    },
    addGroup() {
      this.groups.push({ ...this.newGroup });
      this.newGroup = { name: "", students: [] };
      this.closeModal();
    },
    deleteGroup(index) {
      this.groups.splice(index, 1);
    },
    openAddStudentModal(groupIndex) {
      this.selectedGroupIndex = groupIndex;
      this.modalType = "addStudent";
      this.showModal = true;
    },
    addStudentsToGroup() {
      if (this.selectedGroupIndex !== null) {
        this.groups[this.selectedGroupIndex].students.push(...this.selectedStudents);
        this.selectedStudents = [];
        this.closeModal();
      }
    },
  },
};
</script>

<style scoped>
/* Additional styles */
</style>
