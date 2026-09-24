<template>
  <form class="form" novalidate @submit.prevent="submit">
    <h2 class="form__title">{{ title }}</h2>

    <div class="form__row">
      <label class="form__field">
        <span class="form__label">Имя и фамилия</span>
        <input
          v-model.trim="name"
          class="form__input"
          :class="{ 'form__input--error': errors.name }"
          type="text"
          placeholder="Иван Иванов"
        />
        <span v-if="errors.name" class="form__helper form__helper--error">
          {{ errors.name }}
        </span>
        <span v-else class="form__helper">Имя появится на бейдже участника</span>
      </label>

      <label class="form__field">
        <span class="form__label">E-mail</span>
        <input
          v-model.trim="email"
          class="form__input"
          :class="{ 'form__input--error': errors.email }"
          type="email"
          placeholder="you@example.com"
        />
        <span v-if="errors.email" class="form__helper form__helper--error">
          {{ errors.email }}
        </span>
        <span v-else class="form__helper">Билет придёт на эту почту</span>
      </label>

      <label class="form__field form__field--narrow">
        <span class="form__label">Формат участия</span>
        <select v-model="format" class="form__input">
          <option value="offline">Офлайн, Минск</option>
          <option value="online">Онлайн-трансляция</option>
        </select>
        <span class="form__helper">Онлайн-участие бесплатное</span>
      </label>
    </div>

    <label class="form__checkbox">
      <input v-model="acceptsNews" type="checkbox" />
      <span>Присылать анонсы следующих конференций</span>
    </label>

    <button class="form__submit" type="submit">
      <span>{{ submitLabel }}</span>
      <img class="form__icon" :src="arrowIcon" alt="" />
    </button>
  </form>
</template>

<script>
import arrowIcon from '../assets/icons/icon-arrow-right-white.svg';

const EMAIL_PATTERN = /^[^\s@]+@[^\s@]+\.[^\s@]{2,}$/;

export default {
  name: 'RegistrationForm',

  props: {
    title: {
      type: String,
      default: 'Регистрация',
    },
    submitLabel: {
      type: String,
      default: 'Зарегистрироваться',
    },
  },

  emits: ['register'],

  data() {
    return {
      name: '',
      email: '',
      format: 'offline',
      acceptsNews: false,
      errors: {
        name: '',
        email: '',
      },
      arrowIcon,
    };
  },

  methods: {
    validate() {
      this.errors.name = this.name.length >= 2 ? '' : 'Укажите имя и фамилию';
      this.errors.email = EMAIL_PATTERN.test(this.email) ? '' : 'Введите корректный e-mail';

      return !this.errors.name && !this.errors.email;
    },

    submit() {
      if (!this.validate()) {
        return;
      }

      this.$emit('register', {
        name: this.name,
        email: this.email,
        format: this.format,
        acceptsNews: this.acceptsNews,
      });

      this.reset();
    },

    reset() {
      this.name = '';
      this.email = '';
      this.format = 'offline';
      this.acceptsNews = false;
    },
  },
};
</script>

<style scoped>
/* Форма регистрации. Соответствует набору Input/Registration из ЛР № 17. */

.form {
  display: flex;
  flex-direction: column;
  gap: var(--space-5);
  padding: var(--space-8);
  background-color: var(--color-neutral-0);
  border-radius: var(--radius-xl);
  box-shadow: var(--shadow-card);
}

.form__title {
  font-size: var(--text-h2-size);
  line-height: var(--text-h2-line);
  font-weight: 700;
  letter-spacing: -0.015em;
  color: var(--color-neutral-900);
}

.form__row {
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-4);
}

.form__field {
  display: flex;
  flex: 1 1 260px;
  flex-direction: column;
  gap: var(--space-2);
}

.form__field--narrow {
  flex: 0 1 220px;
}

.form__label {
  font-size: var(--text-body-sm-size);
  line-height: var(--text-body-sm-line);
  font-weight: 500;
  color: var(--color-neutral-700);
}

.form__input {
  width: 100%;
  padding: 14px var(--space-4);
  background-color: var(--color-neutral-0);
  border: 1.5px solid var(--color-neutral-300);
  border-radius: var(--radius-sm);
  font-family: var(--font-family);
  font-size: var(--text-body-size);
  line-height: var(--text-body-line);
  color: var(--color-neutral-900);
  transition:
    border-color var(--motion-fast) var(--easing),
    box-shadow var(--motion-fast) var(--easing);
}

.form__input::placeholder {
  color: var(--color-neutral-500);
}

.form__input:focus {
  outline: none;
  border-color: var(--color-brand-primary);
  box-shadow: var(--focus-ring);
}

.form__input--error {
  background-color: var(--color-semantic-error-subtle);
  border-color: var(--color-semantic-error);
}

.form__helper {
  font-size: var(--text-caption-size);
  line-height: var(--text-caption-line);
  font-weight: 500;
  color: var(--color-neutral-500);
}

.form__helper--error {
  color: var(--color-semantic-error);
}

.form__checkbox {
  display: inline-flex;
  align-items: center;
  gap: var(--space-3);
  font-size: var(--text-body-sm-size);
  line-height: var(--text-body-sm-line);
  color: var(--color-neutral-700);
  cursor: pointer;
}

.form__submit {
  display: inline-flex;
  align-self: flex-start;
  align-items: center;
  gap: var(--space-3);
  padding: 14px var(--space-6) 14px var(--space-7);
  border: none;
  border-radius: var(--radius-md);
  background-color: var(--color-brand-primary);
  font-family: var(--font-family);
  font-size: var(--text-body-size);
  line-height: var(--text-body-line);
  font-weight: 600;
  color: var(--color-neutral-0);
  cursor: pointer;
  transition: background-color var(--motion-fast) var(--easing);
}

.form__submit:hover {
  background-color: var(--color-brand-primary-hover);
}

.form__submit:active {
  background-color: var(--color-brand-primary-pressed);
}

.form__submit:focus-visible {
  outline: none;
  box-shadow: var(--focus-ring);
}

.form__icon {
  width: 24px;
  height: 24px;
}
</style>
