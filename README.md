# Admin Layout - Vue JS component

[![NPM version](https://img.shields.io/npm/v/admin-layout-vue.svg)](https://www.npmjs.com/package/admin-layout-vue)


## Requirements:
- Bootstrap 5
- Vue router (for `<router-link>`)
- Vue 3

## Install:

```bash
npm install admin-layout-vue
```

## Parameters:

| **Property**                    | **Type** | **Required** |
|---------------------------------|----------|--------------|
| brandName                       | String   | Yes          |
| items                           | Array    | Yes          |
| items[].heading                 | String   | No           |
| items[].subItems                | Array    | Yes          |
| items[].subItems[].label        | String   | Yes          |
| items[].subItems[].iconClass    | String   | No           |
| items[].subItems[].route        | Object   | No           |
| items[].subItems[].route.name   | String   | Yes          |
| items[].subItems[].href         | String   | No           |
| items[].subItems[].target       | String   | No           |
| items[].subItems[].active       | Boolean  | No           |
| items[].subItems[].callable     | Function | No           |
| navbarDropdown                  | Object   | No           |
| navbarDropdown.title            | String   | No           |
| navbarDropdown.iconClass        | String   | Yes          |
| navbarDropdown.items            | Array    | Yes          |
| navbarDropdown.items[].label    | String   | Yes          |
| navbarDropdown.items[].callable | Function | Yes          |

## Example

```vue
<template>
  <admin-layout
      brand-name="Example Brand"
      :items="items"
      :navbar-dropdown="navbarDropdown"
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
       * Menu items
       */
      items: [
        {
          subItems: [
            {
              label: 'First Page',
              iconClass: 'bi bi-collection',
              route: {
                name: 'FirstPage',
              },
            },
          ],
        },
        {
          heading: 'Example heading',
          subItems: [
            {
              label: 'Google',
              href: 'https://google.com',
              target: '_blank',
              active: false,
              callable: () => {
                console.log('Callable example.')
              },
            },
          ],
        },
      ],
      /**
       * Navbar dropdown
       */
      navbarDropdown: {
        title: null,
        iconClass: 'bi bi-person-circle',
        items: [
          {
            label: 'Example item',
            callable: () => {
              console.log('Callable dropdown example.')
            },
          },
        ],
      },
    }
  }
}
</script>
```
