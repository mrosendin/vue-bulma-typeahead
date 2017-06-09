# Vue Bulma Typeahead

> A simple Vue.js 2 Bulma CSS-styled component with a typeahead dropdown

View the [demo](https://www.roseware.io/vue-bulma-typeahead/).

## Installation

Install via NPM:

``` bash
npm install vue-bulma-typeahead --save
```

## Usage

**Props**

|Name|Type|Required|Default|Description|
|----|----|--------|-------|-----------|
|source|Array|True|[]|A data source|
|onSelect|Function|True|N/A|Suggestion selected event callback. Parameters: value|
|onChange|Function|True|N/A|Typeahead input value changed event callback. Parameters: value|
|limit|Number|False|Infinity|Max number of suggestions to show in dropdown.|

**Example**

Use like below. See the [example code](https://github.com/roseware/vue-bulma-typeahead/blob/master/src/Demo.vue) in the demo.

``` html
<template>

  <label class="label">Name</label>
  <p class="control has-icons-left">

    <typeahead :source="source" :onSelect="onSelect" :onChange="onChange" :limit="5"></typeahead>

    <span class="icon is-small is-left">
      <i class="fa fa-magic"></i>
    </span>
  </p>

</template>

<script>
import Typeahead from './Typeahead.vue'

export default {
  name: 'demo',
  components: { Typeahead },
  data () {
    return {
      source: ['Alabama', 'Alaska', 'Arizona', 'Arkansas', 'California', 'Delaware', 'Kansas', 'North Carolina', 'South Carolina', 'Washington'],
      value: ''
    }
  },
  methods: {
    onSelect (value) {
      this.value = value
    },
    onChange (value) {
      this.value = value
    }
  }
}
</script>
```

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

For detailed explanation on how things work, consult the [docs for vue-loader](http://vuejs.github.io/vue-loader).
