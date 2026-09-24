<template>
  <article class="card">
    <img
      class="card__photo"
      :src="speaker.photoUrl"
      :alt="'Фотография спикера: ' + speaker.name"
      width="320"
      height="260"
      loading="lazy"
    />

    <div class="card__content">
      <h3 class="card__name">{{ speaker.name }}</h3>
      <p class="card__role">{{ speaker.role }}</p>
      <p class="card__topic">{{ speaker.topic }}</p>

      <!-- Формат участия: три взаимоисключающих состояния -->
      <span v-if="speaker.format === 'offline'" class="card__badge card__badge--offline">
        Офлайн
      </span>
      <span v-else-if="speaker.format === 'online'" class="card__badge card__badge--online">
        Онлайн
      </span>
      <span v-else class="card__badge card__badge--tbd">Формат уточняется</span>

      <div class="card__divider"></div>

      <div class="card__meta">
        <span class="card__meta-item">
          <img class="card__icon" :src="calendarIcon" alt="" />
          {{ speaker.date }}
        </span>
        <span class="card__meta-item">
          <img class="card__icon" :src="locationIcon" alt="" />
          {{ speaker.hall }}
        </span>
      </div>

      <!-- Детальная информация разворачивается по клику -->
      <div v-if="areDetailsVisible" class="card__details">
        <p class="card__bio">{{ speaker.bio }}</p>
        <p class="card__level">Уровень доклада: {{ speaker.level }}</p>
      </div>

      <button class="card__toggle" type="button" @click="toggleDetails">
        {{ toggleLabel }}
      </button>
    </div>
  </article>
</template>

<script>
import calendarIcon from '../assets/icons/icon-calendar-brand.svg';
import locationIcon from '../assets/icons/icon-location-brand.svg';

export default {
  name: 'SpeakerCard',

  props: {
    speaker: {
      type: Object,
      required: true,
      validator(value) {
        return ['id', 'name', 'topic', 'photoUrl'].every((field) => field in value);
      },
    },
  },

  emits: ['toggle'],

  data() {
    return {
      areDetailsVisible: false,
      calendarIcon,
      locationIcon,
    };
  },

  computed: {
    toggleLabel() {
      return this.areDetailsVisible ? 'Скрыть подробности' : 'Подробнее о спикере';
    },
  },

  methods: {
    toggleDetails() {
      this.areDetailsVisible = !this.areDetailsVisible;
      this.$emit('toggle', {
        id: this.speaker.id,
        isVisible: this.areDetailsVisible,
      });
    },
  },
};
</script>

<style scoped>
/* Карточка спикера. Соответствует набору Card/Speaker из ЛР № 17. */

.card {
  display: flex;
  flex-direction: column;
  height: 100%;
  background-color: var(--color-neutral-0);
  border-radius: var(--radius-xl);
  box-shadow: var(--shadow-card);
  overflow: hidden;
  transition:
    box-shadow var(--motion-medium) var(--easing),
    transform var(--motion-medium) var(--easing);
}

.card:hover {
  box-shadow: var(--shadow-card-hover);
  transform: translateY(-4px);
}

.card__photo {
  display: block;
  width: 100%;
  height: 260px;
  object-fit: cover;
  background-color: var(--color-brand-primary);
}

.card__content {
  display: flex;
  flex: 1 1 auto;
  flex-direction: column;
  align-items: flex-start;
  gap: var(--space-2);
  padding: var(--space-5) var(--space-6) var(--space-6);
}

.card__name {
  font-size: var(--text-h3-size);
  line-height: var(--text-h3-line);
  font-weight: 600;
  letter-spacing: -0.01em;
  color: var(--color-neutral-900);
}

.card__role {
  font-size: var(--text-body-sm-size);
  line-height: var(--text-body-sm-line);
  color: var(--color-neutral-500);
}

.card__topic {
  font-size: var(--text-body-size);
  line-height: var(--text-body-line);
  color: var(--color-neutral-700);
}

.card__badge {
  padding: var(--space-1) var(--space-3);
  border-radius: 999px;
  font-size: var(--text-caption-size);
  line-height: var(--text-caption-line);
  font-weight: 600;
}

.card__badge--offline {
  background-color: var(--color-brand-primary-subtle);
  color: var(--color-brand-primary);
}

.card__badge--online {
  background-color: #ecfeff;
  color: #0e7490;
}

.card__badge--tbd {
  background-color: var(--color-neutral-100);
  color: var(--color-neutral-500);
}

.card__divider {
  align-self: stretch;
  height: 1px;
  margin-top: auto;
  background-color: var(--color-neutral-200);
}

.card__meta {
  display: flex;
  align-items: center;
  gap: var(--space-4);
  margin-top: var(--space-2);
  color: var(--color-neutral-500);
}

.card__meta-item {
  display: inline-flex;
  align-items: center;
  gap: var(--space-2);
  font-size: var(--text-caption-size);
  line-height: var(--text-caption-line);
  font-weight: 500;
}

.card__icon {
  width: 18px;
  height: 18px;
}

.card__details {
  align-self: stretch;
  padding: var(--space-3) var(--space-4);
  background-color: var(--color-neutral-100);
  border-radius: var(--radius-sm);
  font-size: var(--text-body-sm-size);
  line-height: var(--text-body-sm-line);
  color: var(--color-neutral-700);
}

.card__bio {
  margin-bottom: var(--space-2);
}

.card__level {
  font-weight: 600;
  color: var(--color-neutral-900);
}

.card__toggle {
  padding: 0;
  background: none;
  border: none;
  font-family: var(--font-family);
  font-size: var(--text-body-sm-size);
  line-height: var(--text-body-sm-line);
  font-weight: 600;
  color: var(--color-brand-primary);
  cursor: pointer;
}

.card__toggle:hover {
  color: var(--color-brand-primary-hover);
}

.card__toggle:focus-visible {
  outline: none;
  box-shadow: var(--focus-ring);
  border-radius: var(--space-1);
}
</style>
