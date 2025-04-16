<script setup>
import { ref } from "vue";

// Список предметов пользователя
const userItems = [
  { id: 1, name: "Shoes 1" },
  { id: 2, name: "Shoes 2" },
  { id: 3, name: "Shoes 3" },
  { id: 4, name: "Shoes 4" },
  { id: 5, name: "T-shirt 1" },
  { id: 6, name: "T-shirt 2" },
  { id: 7, name: "T-shirt 3" },
  { id: 8, name: "T-shirt 4" },
];

// Список доступных предметов
const availableItems = [
  { id: 11, name: "Jacket 1" },
  { id: 12, name: "Jacket 2" },
  { id: 13, name: "Jacket 3" },
  { id: 14, name: "Jacket 4" },
  { id: 15, name: "Hoodie 1" },
  { id: 16, name: "Hoodie 2" },
  { id: 17, name: "Hoodie 3" },
  { id: 18, name: "Hoodie 4" },
];

// Выбранные предметы пользователя (до 6)
const selectedUserItems = ref([]);

// Выбранный доступный предмет (только один)
const selectedAvailableItem = ref(null);

// Функция для выбора предмета пользователя
const selectUserItem = (item) => {
  // Проверяем, не выбран ли предмет уже
  if (selectedUserItems.value.includes(item)) {
    // Если выбран, убираем его
    removeSelectedUserItem(item);
  } else if (selectedUserItems.value.length < 6) {
    // Если меньше 6 предметов, добавляем новый
    selectedUserItems.value.push(item);
  } else {
    // Сообщаем пользователю, что достигнут лимит
    alert("Достигнут лимит в 6 предметов!");
  }
};

// Функция для выбора доступного предмета
const selectAvailableItem = (item) => {
  // Если предмет уже выбран, убираем его, иначе выбираем новый
  selectedAvailableItem.value =
    selectedAvailableItem.value === item ? null : item;
};

// Функция для удаления выбранного предмета пользователя
const removeSelectedUserItem = (item) => {
  const index = selectedUserItems.value.indexOf(item);
  if (index > -1) {
    selectedUserItems.value.splice(index, 1);
  }
};
</script>

<template>
  <div class="container">
    <!-- Верхняя секция: выбранные предметы -->
    <div class="top-section">
      <!-- Выбранные предметы пользователя -->
      <div class="selected-user-items">
        <h3>Выбранные предметы ({{ selectedUserItems.length }}/6)</h3>
        <div class="items-grid">
          <div
            v-for="item in selectedUserItems"
            :key="item.id"
            class="item fade-in"
            @click="removeSelectedUserItem(item)"
          >
            {{ item.name }}
            <span class="remove-icon">×</span>
          </div>
          <!-- Сообщение, если нет выбранных предметов -->
          <div v-if="!selectedUserItems.length" class="item">
            Нет выбранных предметов
          </div>
        </div>
      </div>
      <!-- Выбранный доступный предмет -->
      <div class="selected-available-item">
        <h3>Выбранный предмет</h3>
        <div class="item zoom-in" v-if="selectedAvailableItem">
          {{ selectedAvailableItem.name }}
        </div>
        <!-- Сообщение, если предмет не выбран -->
        <div v-else class="item">Нет выбранного предмета</div>
      </div>
    </div>

    <!-- Нижняя секция: списки предметов -->
    <div class="bottom-section">
      <!-- Предметы пользователя -->
      <div class="user-items">
        <h3>Предметы пользователя</h3>
        <div class="items-grid">
          <div
            v-for="item in userItems"
            :key="item.id"
            class="item"
            :class="{ selected: selectedUserItems.includes(item) }"
            @click="selectUserItem(item)"
          >
            {{ item.name }}
          </div>
          <!-- Сообщение, если список пуст -->
          <div v-if="!userItems.length" class="item">
            Нет доступных предметов пользователя
          </div>
        </div>
      </div>
      <!-- Доступные предметы -->
      <div class="available-items">
        <h3>Доступные предметы</h3>
        <div class="items-grid">
          <div
            v-for="item in availableItems"
            :key="item.id"
            class="item"
            :class="{ selected: selectedAvailableItem === item }"
            @click="selectAvailableItem(item)"
          >
            {{ item.name }}
          </div>
          <!-- Сообщение, если список пуст -->
          <div v-if="!availableItems.length" class="item">
            Нет доступных предметов
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
// Переменные для стилей
$primary-color: #4caf50;
$secondary-color: #2196f3;
$background-dark: #1a1a1a;
$card-background: rgba(255, 255, 255, 0.1);
$hover-background: rgba(255, 255, 255, 0.15);
$text-color: #ffffff;
$border-radius: 12px;
$transition-duration: 0.3s;

// Контейнер страницы
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
  display: flex;
  flex-direction: column;
  gap: 2rem;
  min-height: 100vh;
}

// Стили для верхней и нижней секций
.top-section,
.bottom-section {
  display: flex;
  gap: 2rem;
  justify-content: center;
  flex-wrap: wrap;

  > div {
    flex: 1;
    background: $card-background;
    border-radius: $border-radius;
    padding: 1.5rem;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
    transition: transform $transition-duration ease,
      box-shadow $transition-duration ease;

    &:hover {
      transform: translateY(-3px);
      box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
    }
  }
}

// Стили для заголовков
h3 {
  margin-bottom: 1.5rem;
  color: $text-color;
  font-size: 1.3rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 1px;
  text-align: center;
  background: linear-gradient(45deg, $primary-color, $secondary-color);
  padding: 0.8rem;
  border-radius: $border-radius;
}

// Стили для сетки предметов
.items-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 1rem;
}

// Стили для отдельных предметов
.item {
  background: rgba(255, 255, 255, 0.05);
  border: 2px solid rgba(255, 255, 255, 0.1);
  border-radius: $border-radius;
  padding: 1rem;
  text-align: center;
  cursor: pointer;
  transition: all $transition-duration ease;
  position: relative;
  overflow: hidden;

  &:hover {
    background: $hover-background;
    transform: scale(1.05);
    border-color: rgba(255, 255, 255, 0.3);
  }

  &.selected {
    background: linear-gradient(
      135deg,
      rgba($primary-color, 0.3),
      rgba($secondary-color, 0.3)
    );
    border-color: $primary-color;
    animation: pulse 2s infinite;
  }

  .remove-icon {
    position: absolute;
    top: 5px;
    right: 5px;
    font-size: 1.2rem;
    opacity: 0;
    transition: opacity $transition-duration ease;
  }

  &:hover .remove-icon {
    opacity: 1;
  }
}

// Минимальная высота для секций с выбранными предметами
.selected-user-items,
.selected-available-item {
  min-height: 250px;
}

// Анимации
@keyframes pulse {
  0% {
    box-shadow: 0 0 0 0 rgba($primary-color, 0.4);
  }
  70% {
    box-shadow: 0 0 0 15px rgba($primary-color, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba($primary-color, 0);
  }
}

.fade-in {
  animation: fadeIn 0.5s ease-in;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.zoom-in {
  animation: zoomIn 0.4s ease-out;
}

@keyframes zoomIn {
  from {
    opacity: 0;
    transform: scale(0.8);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

// Глобальные стили
body {
  background: $background-dark;
  color: $text-color;
  font-family: "Inter", sans-serif;
  margin: 0;
  padding: 0;
}

* {
  box-sizing: border-box;
}
</style>
