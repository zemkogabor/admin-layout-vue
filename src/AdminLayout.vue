<template>
  <div ref="wrapper" class="layout-wrapper">
    <nav class="navbar navbar-expand navbar-top border-bottom px-3" :class="topNavClass" :data-bs-theme="dataBsThemeTop">
      <div class="me-auto">
        <button type="button" class="btn d-md-none" @click="showLeftNavbarToggle">
          <i class="bi bi-list" />
        </button>
        <button type="button" class="btn d-none d-md-block" @click="minimizeLeftNavbarToggle">
          <i class="bi bi-list" />
        </button>
      </div>
      <slot name="topEnd" />
      <div class="navbar-nav">
        <div v-for="(item, index) in topItems" :key="index" class="nav-item">
          <button
            class="btn nav-link border-0"
            type="button"
            :data-bs-toggle="item.subItems !== undefined ? 'dropdown' : undefined"
            @click="item.callable"
          >
            <i v-if="item.iconClass" :class="item.iconClass" class="position-relative">
              <span
                v-if="item.showBadge"
                class="position-absolute top-0 start-100 translate-middle p-1 bg-danger border border-light rounded-circle"
              />
            </i>
            <span v-if="item.label" class="ms-2" v-text="item.label" />
          </button>
          <ul v-if="item.subItems" class="dropdown-menu dropdown-menu-end">
            <li v-for="(subItem, subItemIndex) in item.subItems" :key="subItemIndex">
              <button
                class="dropdown-item"
                type="button"
                :class="subItem.class"
                @click="subItem.callable"
              >
                <i v-if="subItem.iconClass !== undefined" :class="subItem.iconClass" />
                {{ subItem.label }}
              </button>
            </li>
          </ul>
        </div>
      </div>
    </nav>
    <nav class="navbar navbar-left" :class="[leftNavClass, leftNavBgClass]" :data-bs-theme="dataBsThemeLeft">
      <div class="navbar-brand">
        <slot name="brand">
          <span v-if="brandName" class="brand-text">
            {{ brandName }}
          </span>
        </slot>
      </div>
      <div class="navbar-nav">
        <div v-for="(group, groupIndex) in leftItemsByGroups" :key="groupIndex" class="group-wrapper">
          <div v-if="group.heading" class="group-heading-text text-uppercase mb-2">
            {{ group.heading }}
          </div>
          <template v-for="(item, itemIndex) in group.items" :key="itemIndex">
            <div v-if="item.visible !== false" class="nav-item position-relative">
              <router-link
                v-if="item.route"
                :to="item.route"
                class="nav-link d-flex align-items-center"
                :class="{
                  'active': isActiveRoute(item.route, item.relatedRoutes),
                }"
                @click="hideLeftNavbarToggle"
              >
                <i class="nav-link-icon" :class="item.iconClass" />
                <span class="nav-link-text text-nowrap" v-text="item.label" />
              </router-link>
              <template v-else-if="item.subItems">
                <a
                  class="nav-link d-flex align-items-center"
                  :class="{
                    'collapsed': !isActiveAnySubItem(item.subItems),
                  }"
                  href="#"
                  data-bs-toggle="collapse"
                  :data-bs-target="'#group' + groupIndex + 'item' + itemIndex"
                >
                  <i class="nav-link-icon" :class="item.iconClass" />
                  <span class="nav-link-text text-nowrap" v-text="item.label" />
                  <i class="nav-link-care-icon bi bi-caret-down-fill ms-auto caret-icon" />
                </a>
                <div
                  :id="'group' + groupIndex + 'item' + itemIndex"
                  class="nav-sub-items collapse"
                  :class="[
                    {'show': isActiveAnySubItem(item.subItems)},
                    leftNavBgClass
                  ]"
                >
                  <template v-for="(subItem, subItemIndex) in item.subItems" :key="subItemIndex">
                    <div v-if="subItem.visible !== false" class="nav-item">
                      <router-link
                        class="nav-link text-nowrap d-flex align-items-center"
                        :class="{
                          'active': isActiveRoute(subItem.route, subItem.relatedRoutes),
                        }"
                        :to="subItem.route"
                        @click="hideLeftNavbarToggle"
                      >
                        {{ subItem.label }}
                      </router-link>
                    </div>
                  </template>
                </div>
              </template>
            </div>
          </template>
        </div>
      </div>
    </nav>
    <main role="main" class="main">
      <slot />
    </main>
    <div class="backdrop" @click="showLeftNavbarToggle" />
  </div>
</template>

<script>
export default {
  name: 'AdminLayout',
  props: {
    brandName: {
      type: String,
      default: undefined,
    },
    topItems: {
      type: Array,
      required: true,
    },
    leftItemsByGroups: {
      type: Array,
      required: true,
    },
    topNavClass: {
      type: [String, Object],
      default: null,
    },
    leftNavClass: {
      type: [String, Object],
      default: null,
    },
    leftNavBgClass: {
      type: [String, Object],
      default: null,
    },
    dataBsThemeTop: {
      type: String,
      default: null,
    },
    dataBsThemeLeft: {
      type: String,
      default: null,
    },
    strictActiveRoute: {
      type: Boolean,
      default: true,
    },
  },
  methods: {
    showLeftNavbarToggle() {
      this.$refs.wrapper.classList.toggle('navbar-left-show')
    },
    hideLeftNavbarToggle() {
      this.$refs.wrapper.classList.remove('navbar-left-show')
    },
    minimizeLeftNavbarToggle() {
      this.$refs.wrapper.classList.toggle('navbar-left-minimize')
    },
    /**
     * @param route
     * @param {?String[]} relatedRoutes
     * @returns {boolean}
     */
    isActiveRoute(route, relatedRoutes) {
      // In strict mode and if query parameter is defined, it must also match to be active.
      if (this.strictActiveRoute && route.query !== undefined) {
        return this.$route.fullPath === this.$router.resolve(route).fullPath
      }

      // In other cases, it is enough if the route name matches.
      return this.$route.name === route.name || (relatedRoutes && relatedRoutes.includes(this.$route.name))
    },
    /**
     * @param subItems
     * @returns {boolean}
     */
    isActiveAnySubItem(subItems) {
      return subItems.some((subItem) => {
        return this.isActiveRoute(subItem.route, subItem.relatedRoutes)
      })
    },
  },
}
</script>

<style lang="scss" scoped>
@import 'bootstrap/scss/functions';
@import 'bootstrap/scss/variables';
@import 'bootstrap/scss/mixins/breakpoints';

$navbar-left-width: 15rem !default;
$navbar-left-width-minimize: 2.5rem !default;
$navbar-left-x-padding: .75rem !default;
$navbar-left-transition-time: .25s !default;
$navbar-left-group-heading-text-font-size: .75rem !default;
$navbar-left-nav-item-height: 2.5rem !default;
$navbar-top-height: 3rem !default;
$main-padding: 1rem !default;

.navbar-left {
  .nav-link {
    height: $navbar-left-nav-item-height;
  }
}

.navbar-top {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: $zindex-fixed;
  height: $navbar-top-height;
  transition: left $navbar-left-transition-time;
}

.navbar-left {
  display: block;
  position: fixed;
  top: 0;
  bottom: 0;
  z-index: $zindex-fixed;
  width: 0;
  transition: width $navbar-left-transition-time;

  .nav-item {
    padding-left: $navbar-left-x-padding;
    padding-right: $navbar-left-x-padding;
  }
}

.main {
  margin-top: $navbar-top-height;
  padding: $main-padding;
  transition: margin-left $navbar-left-transition-time;
}

.backdrop {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  z-index: $zindex-modal-backdrop;
  width: 100vw;
  height: 100vh;
  background-color: $dark;
  opacity: .5;
}

.navbar-brand {
  height: $navbar-top-height;
  padding-left: $navbar-left-x-padding;
  padding-right: $navbar-left-x-padding;
  margin: 0;
}

.nav-link-icon {
  margin-right: .75em;
  // The icon must be just large enough to be in the center when it is displayed in minimized size.
  // (Because of the animation, it doesn't look good when the icon position changes, so we don't use manual centering)
  font-size: $navbar-left-width-minimize - (2 * $navbar-left-x-padding);
}

.nav-sub-items {
  padding-left: $navbar-left-width-minimize - (2 * $navbar-left-x-padding);
}

.group-wrapper {
  margin-bottom: 1rem;
}

.group-heading-text {
  padding-left: $navbar-left-x-padding;
  padding-right: $navbar-left-x-padding;
  font-size: $navbar-left-group-heading-text-font-size;
  cursor: default;
  color: var(--bs-nav-link-color);
  white-space: nowrap;
}

.caret-icon {
  transition: transform .15s;
}

.collapsed {
  .caret-icon {
    transform: rotate(-90deg);
  }
}

@include  media-breakpoint-down(md) {
  .navbar-left {
    overflow: hidden; // This guarantees that at 0 width the content is not visible.
  }

  .navbar-left-show {
    .navbar-left {
      width: $navbar-left-width;
      z-index: $zindex-modal;
    }

    .backdrop {
      display: block;
    }
  }
}

@include media-breakpoint-up(md) {
  .navbar-left {
    width: $navbar-left-width;
  }

  .navbar-top {
    left: $navbar-left-width;
  }
  .main {
    margin-left: $navbar-left-width;
  }

  .navbar-left-minimize {
    .navbar-left {
      width: $navbar-left-width-minimize;
    }
    .navbar-top {
      left: $navbar-left-width-minimize;
    }
    .main {
      margin-left: $navbar-left-width-minimize;
    }

    .brand-text {
      display: none;
    }

    .nav-link-icon {
      margin-right: 0;
    }

    .nav-link-text {
      display: none;
    }

    .nav-link-care-icon {
      display: none;
    }

    .group-wrapper {
      margin-bottom: 0;
    }

    .group-heading-text {
      display: none;
    }

    .nav-sub-items {
      display: none;
      width: $navbar-left-width - $navbar-left-width-minimize;
      position: absolute;
      left: $navbar-left-width-minimize;
      top: 0;
      padding-left: 0;
      border-top: var(--bs-border-width) var(--bs-border-style) var(--bs-border-color);
      border-right: var(--bs-border-width) var(--bs-border-style) var(--bs-border-color);
      border-bottom: var(--bs-border-width) var(--bs-border-style) var(--bs-border-color);

      &.collapsing {
        // This guarantees that the button press does not trigger an animation. (In "minimize" size)
        transition: none;
        height: unset;
      }
    }

    .nav-item:hover {
      .nav-sub-items {
        display: block;
      }
    }
  }
}
</style>
