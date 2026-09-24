<template>
  <div class="page">
    <header class="hero">
      <img class="hero__logo" :src="logo" alt="DEVCONF 2026" />
      <h1 class="hero__title">{{ tagline }}</h1>
      <p class="hero__details">{{ details }}</p>
    </header>

    <SpeakersList :speakers="speakers" title="Спикеры" />

    <section id="registration" class="registration">
      <RegistrationForm @register="handleRegister" />

      <aside class="registrations">
        <h3 class="registrations__title">Зарегистрированные участники</h3>

        <ul v-if="registrations.length" class="registrations__list">
          <li
            v-for="participant in registrations"
            :key="participant.email"
            class="registrations__item"
          >
            <span class="registrations__name">{{ participant.name }}</span>
            <span class="registrations__email">{{ participant.email }}</span>
            <span class="registrations__format">{{ formatLabel(participant.format) }}</span>
          </li>
        </ul>

        <p v-else class="registrations__empty">
          Пока никто не зарегистрировался. Заполните форму слева.
        </p>
      </aside>
    </section>

    <footer class="footer">
      DEVCONF 2026 · Лабораторная работа № 21 · Vue 3, однофайловые компоненты, директивы
    </footer>
  </div>
</template>

<script>
import SpeakersList from './components/SpeakersList.vue';
import RegistrationForm from './components/RegistrationForm.vue';

import logo from './assets/icons/logo-devconf.svg';
import speaker01 from './assets/images/speaker-01.webp';
import speaker02 from './assets/images/speaker-02.webp';
import speaker03 from './assets/images/speaker-03.webp';
import speaker04 from './assets/images/speaker-04.webp';

export default {
  name: 'App',

  components: {
    SpeakersList,
    RegistrationForm,
  },

  data() {
    return {
      logo,
      tagline: 'Конференция инженеров и разработчиков, 18 мая 2026',
      details: 'Минск · 24 доклада · 6 воркшопов · онлайн-трансляция бесплатно',
      registrations: [],
      speakers: [
        {
          id: 1,
          name: 'Алексей Ковалёв',
          role: 'Staff Engineer · EPAM',
          topic: 'Трекинг объектов в реальном времени: от YOLO до DeepSORT',
          photoUrl: speaker01,
          date: '18 мая, 14:00',
          hall: 'Зал A',
          format: 'offline',
          level: 'продвинутый',
          bio: 'Семь лет занимается компьютерным зрением в промышленности, внедрял системы видеоаналитики на производственных площадках.',
        },
        {
          id: 2,
          name: 'Дарья Мельник',
          role: 'Head of Design · Wargaming',
          topic: 'Дизайн-система без боли: что ломается на втором году',
          photoUrl: speaker02,
          date: '18 мая, 15:30',
          hall: 'Зал B',
          format: 'offline',
          level: 'средний',
          bio: 'Собрала и поддерживает дизайн-систему, которой пользуются четыре продуктовые команды.',
        },
        {
          id: 3,
          name: 'Игорь Савчук',
          role: 'SRE · iTechArt',
          topic: 'Наблюдаемость сервисов: метрики, которые правда нужны',
          photoUrl: speaker03,
          date: '19 мая, 11:00',
          hall: 'Зал A',
          format: 'online',
          level: 'средний',
          bio: 'Отвечает за надёжность платформы из полутора сотен сервисов, ведёт курс по мониторингу.',
        },
        {
          id: 4,
          name: 'Никита Ермолович',
          role: 'Tech Lead · Godel Technologies',
          topic: 'Реактивность во Vue: как это устроено внутри',
          photoUrl: speaker04,
          date: '19 мая, 13:20',
          hall: 'Зал C',
          format: 'online',
          level: 'начальный',
          bio: 'Разбирает исходники фронтенд-фреймворков и рассказывает о них простым языком.',
        },
      ],
    };
  },

  methods: {
    handleRegister(participant) {
      this.registrations.push(participant);
    },

    formatLabel(format) {
      return format === 'online' ? 'Онлайн' : 'Офлайн';
    },
  },
};
</script>

<style scoped>
.page {
  min-height: 100vh;
}

.hero {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  gap: var(--space-5);
  min-height: 440px;
  padding: var(--space-16) var(--space-12);
  background-image:
    linear-gradient(
      135deg,
      rgba(15, 23, 42, 0.92) 0%,
      rgba(49, 46, 129, 0.82) 100%
    ),
    url('./assets/images/banner-hero.webp');
  background-size: cover;
  background-position: center;
}

.hero__logo {
  height: 48px;
  width: auto;
}

.hero__title {
  max-width: 900px;
  font-size: var(--text-h1-size);
  line-height: var(--text-h1-line);
  font-weight: 700;
  letter-spacing: -0.02em;
  color: var(--color-neutral-0);
}

.hero__details {
  font-size: var(--text-body-lg-size);
  line-height: var(--text-body-lg-line);
  color: var(--color-neutral-300);
}

.registration {
  display: grid;
  grid-template-columns: minmax(360px, 2fr) minmax(280px, 1fr);
  gap: var(--space-6);
  padding: 0 var(--space-12) var(--space-24);
}

.registrations {
  padding: var(--space-6);
  background-color: var(--color-neutral-0);
  border-radius: var(--radius-xl);
  box-shadow: var(--shadow-card);
}

.registrations__title {
  margin-bottom: var(--space-4);
  font-size: var(--text-h3-size);
  line-height: var(--text-h3-line);
  font-weight: 600;
  color: var(--color-neutral-900);
}

.registrations__list {
  display: flex;
  flex-direction: column;
  gap: var(--space-3);
  margin: 0;
  padding: 0;
  list-style: none;
}

.registrations__item {
  display: flex;
  flex-direction: column;
  gap: var(--space-1);
  padding: var(--space-3) var(--space-4);
  background-color: var(--color-neutral-100);
  border-radius: var(--radius-sm);
}

.registrations__name {
  font-size: var(--text-body-size);
  line-height: var(--text-body-line);
  font-weight: 600;
  color: var(--color-neutral-900);
}

.registrations__email {
  font-size: var(--text-body-sm-size);
  line-height: var(--text-body-sm-line);
  color: var(--color-neutral-500);
}

.registrations__format {
  font-size: var(--text-caption-size);
  line-height: var(--text-caption-line);
  font-weight: 600;
  color: var(--color-brand-primary);
}

.registrations__empty {
  font-size: var(--text-body-sm-size);
  line-height: var(--text-body-sm-line);
  color: var(--color-neutral-500);
}

.footer {
  padding: var(--space-8) var(--space-12);
  border-top: 1px solid var(--color-neutral-200);
  font-size: var(--text-body-sm-size);
  line-height: var(--text-body-sm-line);
  color: var(--color-neutral-500);
}

@media (max-width: 900px) {
  .hero,
  .footer {
    padding: var(--space-12) var(--space-6);
  }

  .registration {
    grid-template-columns: 1fr;
    padding: 0 var(--space-6) var(--space-12);
  }

  .hero__title {
    font-size: var(--text-h2-size);
    line-height: var(--text-h2-line);
  }
}
</style>
