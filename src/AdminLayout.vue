<template>
  <nav class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0 shadow">
    <div class="navbar-brand col-md-3 col-lg-2 d-none d-md-block px-3 me-auto">
      {{ brandName }}
    </div>
    <button
      type="button"
      class="navbar-toggler d-md-none collapsed me-auto"
      data-bs-toggle="collapse"
      data-bs-target="#sidebarMenu"
      aria-controls="sidebarMenu"
      aria-expanded="false"
    >
      <span class="navbar-toggler-icon" />
    </button>
    <slot name="navbar-end" />
  </nav>

  <div class="container-fluid">
    <div class="row">
      <nav id="sidebarMenu" class="col-md-3 col-lg-2 d-md-block bg-dark sidebar collapse collapse-horizontal">
        <div class="position-sticky sidebar-content">
          <ul class="nav flex-column">
            <template v-for="(item, index) in items" :key="index">
              <div v-if="item.heading" class="text-uppercase px-3 mt-3 text-light small">
                {{ item.heading }}
              </div>
              <li v-for="(subItem, subItemIndex) in item.subItems" :key="subItemIndex" class="nav-item">
                <router-link
                  v-if="subItem.route"
                  :to="subItem.route"
                  class="nav-link"
                  :class="{
                    'text-light': subItem.route.name === $route.name,
                    'text-muted': subItem.route.name !== $route.name
                  }"
                >
                  <sidebar-sub-item :label="subItem.label" :icon-class="subItem.iconClass" />
                </router-link>
                <a
                  v-else
                  class="nav-link"
                  :class="{
                    'text-primary': subItem.active
                  }"
                  :href="subItem.href"
                  :target="subItem.target"
                  @click="subItem.callable"
                >
                  <sidebar-sub-item :label="subItem.label" :icon-class="subItem.iconClass" />
                </a>
              </li>
            </template>
          </ul>
        </div>
      </nav>
      <main role="main" class="col-md-9 col-lg-10 ms-sm-auto pe-4">
        <div class="py-3">
          <slot />
        </div>
      </main>
    </div>
  </div>
</template>

<script>
import SidebarSubItem from './SidebarSubItem.vue'

/**
 * @typedef {Object} SidebarItem
 * @property {string|null} heading - Sidebar item heading.
 * @property {SubItem[]} subItems - Sidebar item sub items.
 *
 * @typedef {Object} SubItem
 * @property {string} label - Sub item label.
 * @property {string|null} iconClass - Sub item icon class.
 * @property {Route|null} route
 * @property {string|null} href - Sub item href.
 * @property {string|null} target - Sub item target, e.g.: "_blank"
 * @property {boolean|null} active - Sub item active or not.
 * @property {function|null} callable - Callable function onclick

 * @typedef {Object} Route
 * @property {string} name - Route name.
 *
 * @typedef {Object} NavbarDropdown
 * @param {string|null} title - Title
 * @param {string} iconClass - Icon class
 * @param {NavbarDropdownItem[]} items - Dropdown items.
 *
 * @typedef {Object} NavbarDropdownItem
 * @param {string} label - Label
 * @param {function} callable - Callable function onclick
 */
export default {
  name: 'AdminLayout',
  components: {
    SidebarSubItem,
  },
  props: {
    brandName: {
      type: String,
      required: true,
    },
    items: {
      /** @type {SidebarItem[]} */
      type: Array,
      required: true,
    },
    navbarDropdown: {
      /** @type {NavbarDropdown} */
      type: Object,
      default: null,
    },
  },
}
</script>

<style lang="scss" scoped>
.sidebar {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  z-index: 100; /* Behind the navbar */
  padding-top: 3.6rem;
  box-shadow: inset -1px 0 0 rgba(0, 0, 0, .1);

  // only SM
  @media (min-width: 576px) and (max-width: 768px) {
    width: 12rem;
  }

  .sidebar-content {
    width: 11.25rem;
  }
}

.navbar {
  height: 3rem;
}

.navbar-brand {
  padding-top: .75rem;
  padding-bottom: .75rem;
}

.navbar-toggler {
  border: none;
}

.navbar-toggler:focus {
  box-shadow: none;
}

</style>
