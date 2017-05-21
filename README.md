# Using Filters in Vue.js v2

## Introduction

This application is a basic implementation of filters in Vue.js v2. The app is based on the tutorial found [here](https://coligo.io/vuejs-filters).

## Thoughts

1. The tutorial was outdated and I believe that Vue.js v1 was being used because when I tried to run the given code, I got an error message saying `Failed to resolve filter: capitalize`. Upon looking at the Vue docs and StackOverflow, I found out that built-in filters are no longer supported in v2. So I had to make my own `capitalize` and `currency` filters and add them to the `filters` object in the Vue component.
2. Creating the custom filters was a good exercise - especially for the `currency` filter. Having to think about the edge-cases was a valuable task.
3. I also learned that filters can only be used within the mustache interpolations and `v-bind` expressions. The tutorial called for processing through filters in the `v-for:"product in products"` directive which caused errors when I tried to run the code as well. I had to extract the logic of the `v-for` filters to the Computed properties object.
4. In the Computed properties object, I added a `filtered()` function which initially returned the filtered products based on the user selection from a `<select></select>` element. 
5. From this, I added the orderBy `<select></select>` element and implemented the logic in the `filtered()` function.
6. Since the `products` array is an array of objects, I had to implement my own sorting function to extract the names and prices for the products and sort accordingly. This was surprisingly straightforward.