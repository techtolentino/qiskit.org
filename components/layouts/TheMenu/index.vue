<template>
  <div class="menu">
    <!-- <section class="menu__mobile" tabindex="-1">
      <div class="menu__mobile-inner-container">
        <AppLink
          class="
            menu__link
            menu__home-link
          "
          v-bind="homeLink"
        >
          <AppLogo
            class="menu__logo"
            :class="{ 'menu__logo_active': isActiveHome(homeLink) }"
          />
        </AppLink>
        <label
          class="
            menu__hamburger-toggle
            menu__hamburger-toggle_menu-hidden
          "
          for="mobile-menu-toggle"
        >
          <component :is="isMobileMenuVisible ? 'Close20' : 'Menu20'" />
        </label>
      </div>
      <input
        id="mobile-menu-toggle"
        v-model="isMobileMenuVisible"
        class="menu__mobile-menu-toggle"
        type="checkbox"
      >
      <MobileMenu
        class="menu__mobile-menu"
        :class="{ 'menu__mobile-menu_visible': isMobileMenuVisible }"
      />
    </section>
    <section class="menu__main-level">
      <nav class="menu__navigation-level">
        <AppLink
          class="
            menu__link
            menu__home-link
          "
          v-bind="homeLink"
        >
          <AppLogo
            class="menu__logo"
            :class="{ 'menu__logo_active': isActiveHome(homeLink) }"
          />
        </AppLink>
        <AppLink
          v-for="link in mainLevelLinks"
          :key="link.url"
          class="menu__link"
          :class="{ 'menu__link_active': isActive(link) }"
          v-bind="link"
        >
          {{ link.label }}
        </AppLink>
      </nav>
    </section>
    <section
      v-if="isCommunityActive()"
      class="menu__second-level"
    >
      <nav class="menu__navigation-level">
        <AppLink
          v-for="link in communitySubLinks"
          :key="link.url"
          class="menu__link"
          :class="{ 'menu__link_active': isActive(link) }"
          v-bind="link"
        >
          {{ link.label }}
        </AppLink>
      </nav>
    </section> -->

    <cv-header aria-label="Carbon header" class="menu" tabindex="-1">
      <cv-header-menu-button aria-label="Header menu" aria-controls="side-nav" />
      <cv-skip-to-content href="#main-content">
        Skip to content
      </cv-skip-to-content>
      <div class="menu__container">
        <cv-header-name href="/">
          <AppLogo
            class="menu__logo"
            :class="{ 'menu__logo_active': isActiveHome(homeLink) }"
          />
        </cv-header-name>
        <cv-header-nav aria-label="Carbon nav">
          <cv-header-menu-item class="menu__link">
            Overview
          </cv-header-menu-item>
          <cv-header-menu-item class="menu__link">
            Learn
          </cv-header-menu-item>
          <cv-header-menu class="menu__link" aria-label="Community" title="Community">
            <cv-header-menu-item class="menu__link">
              Events
            </cv-header-menu-item>
            <cv-header-menu-item class="menu__link">
              Advocates
            </cv-header-menu-item>
          </cv-header-menu>
          <cv-header-menu-item class="menu__link">
            Documentation
          </cv-header-menu-item>
        </cv-header-nav>
      </div>
      <template v-if="true" v-slot:right-panels />
    </cv-header>
  </div>
</template>

<script lang="ts">
import { Watch, Component, Mixins } from 'vue-property-decorator'
import MobileMenu from '~/components/layouts/TheMenu/MobileMenu.vue'
import AppLogo from '~/components/ui/AppLogo.vue'
import AppLink from '~/components/ui/AppLink.vue'
import MenuMixin from '~/mixins/menu'

@Component({
  components: { MobileMenu, AppLink, AppLogo }
})
export default class extends Mixins(MenuMixin) {
  isMobileMenuVisible: boolean = false

  @Watch('isMobileMenuVisible')
  toggleScroll () {
    this.$emit('changeVisibility', this.isMobileMenuVisible ? 'shown' : 'hidden')
    if (this.isMobileMenuVisible) {
      this.$root.$el.classList.add('no-scroll')
    } else {
      this.$root.$el.classList.remove('no-scroll')
    }
  }

  @Watch('$route')
  hideMenu () {
    this.isMobileMenuVisible = false
  }
}
</script>

<style lang="scss">
@import '~carbon-components/scss/globals/scss/typography';

.menu {
  background-color: white;
  top: 3.45rem;
  border-bottom: none;
  height: 3.45rem;

  &__container {
    @include contained();
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  // overrides
  .bx--header__nav::before {
    display: none;
  }

  .bx--header__name {
    padding: 0;

    &:focus {
      border: 1px solid $purple-70;
    }
  }

  .bx--header__menu-item {
    color: $white-text-01;
    padding: $spacing-05;
    border: none;

    &:hover {
      background-color: transparent;
      color: $white-text-01;
      text-decoration: underline;
      cursor: pointer;
    }
  }

  .bx--header__submenu {
    &:hover {
      background-color: $cool-gray-20;
    }
  }

  .bx--header__submenu svg,
  .bx--header__menu-item:hover > svg {
    fill: $white-text-01;
  }

  .bx--header__submenu a:hover {
    cursor: default;
    text-decoration: none;
  }

  .bx--header__menu-title[aria-expanded='true'],
  .bx--header__menu-title[aria-expanded='true'] + .bx--header__menu,
  .bx--header__menu-title[aria-expanded='true'] + .bx--header__submenu {
    background-color: $cool-gray-20;
  }

  .bx--header__menu-title[aria-expanded='true'] + .bx--header__menu .bx--header__menu-item:hover {    background-color: $cool-gray-20;
    text-decoration: underline;
    cursor: pointer;
  }




  &__main-level {
    --link-color: #{$gray-80};
  }

  &__second-level {
    --link-color: #{$inverse-01};
    background-color: $purple-40;
  }

  &__mobile {
    position: relative;
    fill: $purple-70;

    @include mq($from: large) {
      display: none;
    }

    &:focus {
      outline: none;
    }
  }

  &__mobile-menu-toggle {
    // remove from page flow and hid
    visibility: hidden;
    position: absolute;
  }

  .menu__mobile-menu {
    position: fixed;
    top: 3.5rem; // taking into account the height of the top menu
    left: 0;
    bottom: 0;
    right: 0;
    z-index: 150;

    visibility: hidden;
    pointer-events: none;

    &_visible {
      visibility: visible;
      pointer-events: all;
    }
  }

  &__mobile-inner-container,
  &__navigation-level {
    @include contained();
    padding-top: $spacing-05;
    padding-bottom: $spacing-05;
    display: flex;
    justify-content: flex-end;
  }

  &__navigation-level {
    @include mq($until: large) {
      display: none;
    }
  }

  &__logo {
    height: 1.5rem;
    width: auto;
    color: $cool-gray-60;

    &_active {
      color: $purple-70;
    }
  }

  &__link {
    display: inline-flex;
    flex-direction: column;
    justify-content: center;
    margin-right: $spacing-03;

    &:last-child {
      margin-right: 0;
    }

    a {
      @include type-style('body-long-02');

      &_active {
        color: $purple-70;
      }
    }
  }

  &__home-link {
    margin-left: 0;
    margin-right: auto;

    &:hover {
      text-decoration: none;
    }
  }

  &__hamburger-toggle {
    display: inline-flex;
    flex-direction: column;
    justify-content: center;
  }
}
</style>
