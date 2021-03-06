<template>
  <header class="seminar-series-header">
    <div class="seminar-series-header__container">
      <div class="seminar-series-header__main">
        <div>
          <AppPageHeaderTitle>
            Quantum Information Science Seminar Series
          </AppPageHeaderTitle>
          <div class="seminar-series-header__description">
            <p>The Qiskit Quantum Information Science Seminar Series is dedicated to the research and academic communities as a broad and deep dive into the latest cutting edge quantum research.</p>
            <p>The seminar is live and interactive, you can discuss and ask questions as you watch, and is streamed on YouTube.</p>
            <p>Join us live every Friday at 12:00 PM ET.</p>
          </div>
        </div>
        <AppCta v-bind="cta" class="seminar-series-header__cta" />
      </div>
      <div class="seminar-series-header__aside">
        <div class="seminar-series-header__card-up-title-wrapper">
          <div class="seminar-series-header__card-up-title copy__subtitle">
            {{ cardTitle }}
          </div>
        </div>
        <EventCard v-bind="cardContent" :title="cardContent.speaker" :segment="segment" vertical-layout>
          {{ cardContent.title }}
        </EventCard>
      </div>
    </div>
  </header>
</template>

<script lang="ts">
import Vue from 'vue'
import { Component, Prop } from 'vue-property-decorator'
import { SEMINAR_SERIES_ALL_EPISODES_CTA } from '~/constants/appLinks.ts'
import { SeminarSeriesEvent } from '~/hooks/event-conversion-utils.ts'

@Component
export default class SeminarSeriesHeader extends Vue {
  @Prop({ type: Object, required: false }) nextEvent!: SeminarSeriesEvent|null
  @Prop({ type: Array, required: true }) pastEvents!: SeminarSeriesEvent[]

  cta = SEMINAR_SERIES_ALL_EPISODES_CTA
  hasNextEvent = this.nextEvent
  cardTitle = this.hasNextEvent ? 'Up next:' : 'Featured seminar:'
  randomNumber = Math.random()
  randomIndex = Math.floor(this.randomNumber * this.pastEvents.length)
  cardContent = this.hasNextEvent ? this.nextEvent : this.pastEvents[this.randomIndex]
  segment = { action: 'seminar-series > header > talk-on-youtube' }
}
</script>

<style lang="scss" scoped>
@import '~carbon-components/scss/globals/scss/typography';
@import '~/assets/scss/blocks/copy.scss';

.seminar-series-header {
  @include responsive-grid-bg-strip('/images/grid/grid-hero-learn.svg', auto, 28rem);

  &__container {
    @include contained();
    display: grid;
    column-gap: $spacing-07;
    grid-template-columns: 3fr 4fr 3fr;
    grid-template-areas: 'main main aside';
    row-gap: $spacing-07;

    @include mq($until: large) {
      grid-template-columns: 1fr 1fr;
      grid-template-areas: 'main aside';
    }

    @include mq($until: medium) {
      grid-template-columns: 1fr;
      grid-template-areas:
        'main'
        'aside';
    }
  }

  &__aside {
    grid-area: aside;
  }

  &__cta {
    margin: 0 0 $layout-02;

    @include mq($until: medium) {
      margin-top: $layout-03;
    }
  }

  &__description {
    margin: $layout-05 0 0;
    max-width: 6 * $column-size-large;

    > p {
      @include type-style('body-long-01');

      &:not(:last-child) {
        margin-bottom: $spacing-06;
      }
    }

    @include mq($from: medium, $until: large) {
      margin-top: $layout-03;
    }
  }

  &__main {
    display: flex;
    flex-flow: column;
    grid-area: main;
    justify-content: space-between;
    margin-top: $spacing-07;
  }

  &__card-up-title {
    border-bottom: 4px solid $purple-60;
    display: inline;
    padding-bottom: $spacing-02;
    padding-right: $spacing-03;

    &-wrapper {
      margin-bottom: $spacing-06;
    }
  }
}
</style>
