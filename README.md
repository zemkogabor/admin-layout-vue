# Admin Layout - Vue JS component

[![NPM version](https://img.shields.io/npm/v/admin-layout-vue.svg)](https://www.npmjs.com/package/admin-layout-vue)


## Requirements:
- Bootstrap 5.2
- Vue router (for `<router-link>`)
- Vue 3

## Install:

```bash
npm install admin-layout-vue
```

## Parameters:

| **Property**                                      | **Type** | **Required** |
|---------------------------------------------------|----------|--------------|
| brandName                                         | String   | Yes          |
| leftItemsByGroups                                 | Array    | Yes          |
| leftItemsByGroups[].heading                       | String   | No           |
| leftItemsByGroups[].items                         | Array    | Yes          |
| leftItemsByGroups[].items[].label                 | String   | Yes          |
| leftItemsByGroups[].items[].iconClass             | String   | No           |
| leftItemsByGroups[].items[].route                 | Object   | No           |
| leftItemsByGroups[].items[].route.name            | String   | Yes          |
| leftItemsByGroups[].items[].visible               | Bool     | No           |
| leftItemsByGroups[].items[].subItems              | Array    | No           |
| leftItemsByGroups[].items[].subItems[].label      | String   | Yes          |
| leftItemsByGroups[].items[].subItems[].route      | Object   | Yes          |
| leftItemsByGroups[].items[].subItems[].route.name | Object   | Yes          |
| leftItemsByGroups[].items[].subItems[].visible    | Bool     | No           |
| topItems                                          | Array    | Yes          |
| topItems[].label                                  | String   | No           |
| topItems[].iconClass                              | String   | No           |
| topItems[].callable                               | Callable | No           |
| topItems[].showBadge                              | Bool     | No           |
| topItems[].subItems[]                             | Array    | No           |
| topItems[].subItems[].label                       | String   | Yes          |
| topItems[].subItems[].class                       | String   | No           |
| topItems[].subItems[].iconClass                   | String   | No           |
| topItems[].subItems[].callable                    | Callable | Yes          |


## Example

```vue
<template>
  <admin-layout
      brand-name="Brand"
      :left-items-by-groups="leftItemsByGroups"
      :top-items="topItems"
  >
    <router-view />
  </admin-layout>
</template>

<script>
import AdminLayout from 'admin-layout-vue'

export default {
  name: 'ExampleComponent',
  components: {
    AdminLayout,
  },
  data() {
    return {
      /**
       * Sidebar menu items
       */
      leftItemsByGroups: [
        {
          items: [
            {
              label: 'Home',
              iconClass: 'bi bi-house-door-fill',
              route: {
                name: 'home',
              },
            },
          ],
        },
        {
          heading: 'First heading',
          items: [
            {
              label: 'People',
              iconClass: 'bi bi-people-fill',
              route: {
                name: 'people',
              },
            },
            {
              label: 'Briefcase',
              iconClass: 'bi bi-briefcase-fill',
              subItems: [
                {
                  label: 'Option 1',
                  route: {
                    name: 'option',
                    query: {
                      status: '1',
                    },
                  },
                },
                {
                  label: 'Option 2',
                  route: {
                    name: 'option',
                    query: {
                      status: '2',
                    },
                  },
                },
              ],
            },
          ],
        },
        {
          items: [
            {
              label: 'Coin',
              iconClass: 'bi bi-coin',
              route: {
                name: 'coin',
              },
            },
          ],
        },
      ],
      /**
       * Top navbar items
       */
      topItems: [
        {
          iconClass: 'bi bi-person-circle',
          subItems: [
            {
              label: this.$t('logout.submit'),
              callable: this.onLogoutClick,
            },
          ],
          showBadge: false,
        },
        {
          iconClass: 'bi bi-bell-fill',
          callable: this.showNotifications,
          showBadge: this.notificationsTotalCount > 0,
        },
      ],
    }
  }
}
</script>
```

## Screenshots
<img width="1132" alt="Screenshot 2022-12-01 at 15 19 19" src="https://user-images.githubusercontent.com/39522338/205076785-d5b4acd2-8a40-40c0-b033-458bf9999fc5.png">

<img width="1132" alt="Screenshot 2022-12-01 at 15 19 37" src="https://user-images.githubusercontent.com/39522338/205076815-27532683-a0c0-4335-a13a-9b5951c35fb0.png">

<img width="572" alt="Screenshot 2022-12-01 at 15 18 55" src="https://user-images.githubusercontent.com/39522338/205076863-98b33cf8-260f-44a3-b162-e55cd7a4c2a4.png">

