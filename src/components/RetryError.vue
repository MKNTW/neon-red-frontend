<template>
  <div v-if="show" class="retry-error">
    <div class="retry-error-content">
      <span class="retry-icon">⚠️</span>
      <div class="retry-text">
        <strong>{{ message }}</strong>
        <p>Попробуйте повторить запрос</p>
      </div>
      <button @click="handleRetry" class="retry-btn" :disabled="retrying">
        {{ retrying ? 'Повтор...' : 'Повторить' }}
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps({
  show: {
    type: Boolean,
    default: false
  },
  message: {
    type: String,
    default: 'Ошибка соединения с сервером'
  }
})

const emit = defineEmits(['retry'])

const retrying = ref(false)

async function handleRetry() {
  retrying.value = true
  emit('retry')
  // Сбрасываем состояние через небольшую задержку
  setTimeout(() => {
    retrying.value = false
  }, 1000)
}
</script>

<style scoped>
.retry-error {
  background: linear-gradient(135deg, rgba(255, 0, 51, 0.15), rgba(255, 51, 102, 0.15));
  border: 2px solid var(--neon-red);
  border-radius: 12px;
  padding: 16px 20px;
  margin: 16px 0;
  animation: slideDown 0.3s ease;
}

@keyframes slideDown {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.retry-error-content {
  display: flex;
  align-items: center;
  gap: 12px;
}

.retry-icon {
  font-size: 1.5rem;
  flex-shrink: 0;
}

.retry-text {
  flex: 1;
}

.retry-text strong {
  display: block;
  color: var(--neon-red);
  font-size: 0.95rem;
  margin-bottom: 4px;
}

.retry-text p {
  margin: 0;
  color: var(--text-secondary);
  font-size: 0.85rem;
}

.retry-btn {
  padding: 10px 20px;
  background: var(--neon-red);
  color: white;
  border: none;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s;
  font-family: inherit;
  flex-shrink: 0;
}

.retry-btn:hover:not(:disabled) {
  background: var(--neon-pink);
  box-shadow: 0 0 15px rgba(255, 0, 51, 0.4);
  transform: translateY(-2px);
}

.retry-btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}
</style>

