# Vue Bulma Typeahead

[![npm version](https://badge.fury.io/js/vue-bulma-typeahead.svg)](https://badge.fury.io/js/vue-bulma-typeahead)

> A simple Vue.js 2 Bulma CSS-styled component with a typeahead dropdown

View the [demo](https://rosendin.github.io/vue-bulma-typeahead/).

## Installation

Install via NPM:

``` bash
npm install vue-bulma-typeahead --save
```

## Usage

**Props**

|Name|Type|Required|Default|Description|
|----|----|--------|-------|-----------|
|source|Array|True|[]|A data source. Must contain strings.|
|onSelect|Function|True|N/A|Suggestion selected event callback. Parameters: value, name|
|onChange|Function|True|N/A|Typeahead input value changed event callback. Parameters: value, name|
|limit|Number|False|5|Max number of suggestions to show in dropdown.|
|name|String|False|N/A|Name of the typeahead.|
|async|Boolean|False|True|Whether to debounce the suggestions or not.|
|placeholder|String|False|`""`|Placeholder value for input field|

**Example**

Use like below. See the [example code](https://github.com/roseware/vue-bulma-typeahead/blob/master/src/Demo.vue) in the demo.

``` html
<template>

  <div class="container">
    <label class="label">U.S. State</label>
    <p class="control has-icons-left">

      <typeahead :source="source" :onSelect="onSelect" :onChange="onChange" :limit="5"></typeahead>

      <span class="icon is-small is-left">
        <i class="fa fa-magic"></i>
      </span>
    </p>
  </div>

</template>

<script>
import Typeahead from 'vue-bulma-typeahead'

export default {
  name: 'demo',
  components: { Typeahead },
  data () {
    return {
      source: ['Alabama', 'Alaska', 'Arizona', 'Arkansas', 'California',
      'Colorado', 'Connecticut', 'Delaware', 'Florida', 'Georgia', 'Hawaii',
      'Idaho', 'Illinois', 'Indiana', 'Iowa', 'Kansas', 'Kentucky', 'Louisiana',
      'Maine', 'Maryland', 'Massachusetts', 'Michigan', 'Minnesota', 'Mississippi',
      'Montana', 'Nebraska', 'Nevada', 'New Hampshire', 'New Jersey', 'New Mexico',
      'New York', 'North Carolina', 'North Dakota', 'Ohio', 'Oklahoma', 'Oregon',
      'Pennsylvania', 'Rhode Island', 'South Carolina', 'South Dakota', 'Tennessee',
      'Texas', 'Utah', 'Vermont', 'Virginia', 'Washington', 'West Virginia',
      'Wisconsin', 'Wyoming'],
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
