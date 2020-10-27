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

    <cv-header aria-label="Carbon header" class="menu">
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
      <cv-header-menu-item href="javascript:void(0)">
        Link 1
      </cv-header-menu-item>
      <cv-header-menu-item href="javascript:void(0)">
        Link 2
      </cv-header-menu-item>
      <cv-header-menu-item href="javascript:void(0)">
        Link 3
      </cv-header-menu-item>
      <cv-header-menu aria-label="Link 4" title="Link 4">
        <cv-header-menu-item href="javascript:void(0)">
          Submenu Link 1
        </cv-header-menu-item>
        <cv-header-menu-item href="javascript:void(0)">
          Submenu Link 2
        </cv-header-menu-item>
        <cv-header-menu-item href="javascript:void(0)">
          Submenu Link 3
        </cv-header-menu-item>
      </cv-header-menu>
    </cv-header-nav>
    </div>

        <template v-slot:left-panels v-if="true">

      <cv-side-nav id="side-nav" fixed>
        <cv-side-nav-items>
          <cv-header-side-nav-items>
            <cv-header-menu-item href="javascript:void(0)">
            Link 1
          </cv-header-menu-item>
          <cv-header-menu-item href="javascript:void(0)">
            Link 2
          </cv-header-menu-item>
          <cv-header-menu-item href="javascript:void(0)">
            Link 3
          </cv-header-menu-item>
          <cv-header-menu aria-label="Link 4" title="Link 4" :hover-toggle="false">
            <cv-header-menu-item href="javascript:void(0)">
              Submenu Link 1
            </cv-header-menu-item>
            <cv-header-menu-item href="javascript:void(0)">
              Submenu Link 2
            </cv-header-menu-item>
            <cv-header-menu-item href="javascript:void(0)">
              Submenu Link 3
            </cv-header-menu-item>
          </cv-header-menu>
        </cv-header-side-nav-items>
        </cv-side-nav-items>
      </cv-side-nav>
        </template>
        <template v-slot:right-panels v-if="true">

      </template>
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

<style lang="scss" scoped>
@import '~carbon-components/scss/globals/scss/typography';

.menu {
  background-color: white;
  top: 3rem;
  color: $white-text-01;

  &__container {
    @include contained();
    width: 100%;
    display: flex;
    justify-content: space-between;
  }

  // overrides
  .bx--header__nav::before {
    display: none;
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
    @include type-style('body-long-02');
    display: inline-flex;
    flex-direction: column;
    justify-content: center;
    color: var(--link-color);
    text-decoration: none;
    margin-right: $spacing-09;

    &:hover {
      text-decoration: underline;
    }

    &:last-child {
      margin-right: 0;
    }

    &_active {
      color: $purple-70;
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
