<template>
  <section id="speakers" class="speakers">
    <div class="speakers__head">
      <h2 class="speakers__title">{{ title }}</h2>
      <p class="speakers__subtitle">
        Докладов в программе: {{ speakers.length }}. Раскрыто карточек:
        {{ expandedCount }}
      </p>
    </div>

    <div v-if="speakers.length" class="speakers__grid">
      <SpeakerCard
        v-for="speaker in speakers"
        :key="speaker.id"
        :speaker="speaker"
        @toggle="handleToggle"
      />
    </div>

    <p v-else class="speakers__empty">Программа конференции пока формируется.</p>
  </section>
</template>

<script>
import SpeakerCard from './SpeakerCard.vue';

export default {
  name: 'SpeakersList',

  components: {
    SpeakerCard,
  },

  props: {
    speakers: {
      type: Array,
      required: true,
    },
    title: {
      type: String,
      default: 'Спикеры',
    },
  },

  data() {
    return {
      expandedIds: [],
    };
  },

  computed: {
    expandedCount() {
      return this.expandedIds.length;
    },
  },

  methods: {
    handleToggle({ id, isVisible }) {
      if (isVisible) {
        this.expandedIds.push(id);
      } else {
        this.expandedIds = this.expandedIds.filter((item) => item !== id);
      }
    },
  },
};
</script>

<style scoped>
.speakers {
  padding: var(--space-24) var(--space-12);
}

.speakers__head {
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
  margin-bottom: var(--space-8);
}

.speakers__title {
  font-size: var(--text-h2-size);
  line-height: var(--text-h2-line);
  font-weight: 700;
  letter-spacing: -0.015em;
  color: var(--color-neutral-900);
}

.speakers__subtitle {
  font-size: var(--text-body-lg-size);
  line-height: var(--text-body-lg-line);
  color: var(--color-neutral-500);
}

.speakers__grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: var(--space-6);
  align-items: stretch;
}

.speakers__empty {
  padding: var(--space-8);
  background-color: var(--color-neutral-0);
  border-radius: var(--radius-lg);
  color: var(--color-neutral-500);
}

@media (max-width: 900px) {
  .speakers {
    padding: var(--space-12) var(--space-6);
  }

  .speakers__title {
    font-size: var(--text-h3-size);
    line-height: var(--text-h3-line);
  }
}
</style>
