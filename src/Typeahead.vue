<template>
  <span class="vbta">
    <input :class="['input', 'vbta-hint', { visible: matches.length }]" type="text" :value="hint" :placeholder="placeholder" readonly>
    <input v-model="query" class="input vbta-input" type="text" @keyup.delete="handleDelete($event)" @keydown.down.prevent="handleKeyDown($event)" @keydown.up.prevent="handleKeyUp" @keyup.enter.prevent.submit="emitSelect(matches[preselected])">
    <div :class="['vbta-menu', { visible: matches.length && !selected }]">
      <ul>
        <li v-for="match in matches" class="vbta-suggestion" @click="emitSelect(match)">
          <span v-html="match"></span>
        </li>
      </ul>
    </div>
  </span>
</template>

<script>
import debounce from 'lodash.debounce'

export default {
  name: 'typeahead',
  props: {
    source: {
      type: Array,
      default: () => { return [] },
      required: true
    },
    onSelect: {
      type: Function,
      required: true
    },
    onChange: {
      type: Function,
      required: true
    },
    limit: {
      type: Number,
      default: 5,
      required: false
    },
    name: {
      type: String,
      required: false
    },
    async: {
      type: Boolean,
      default: true,
      required: false
    },
    placeholder: {
      type: String,
      default() { return '' },
      required: false
    }
  },
  data () {
    return {
      query: '',
      matches: [],
      hint: '',
      selected: false,
      firstTouch: true,
      preselected: 0
    }
  },
  watch: {
    query: function (value) {
      if (this.async) {
        debounce(this.getMatches, 150)(value)
      } else {
        this.getMatches(value)
      }
      this.onChange(value, this.name)
    }
  },
  methods: {
    handleDelete () {
      this.selected = false
      this.preselected = 0
      this.firstTouch = true
    },
    handleKeyUp () {
      if (this.matches && this.preselected != 0) {
        this.preselected--
        let el = document.getElementsByClassName("vbta-suggestion")[this.preselected]
        el.style['background-color'] = '#00d1b2'
        
        let prev = document.getElementsByClassName("vbta-suggestion")[this.preselected + 1]
        prev.style['background-color'] = '#ffffff'
      }
    },
    handleKeyDown () {
      if (this.matches && this.preselected != this.matches.length - 1) {
        if (!this.firstTouch) {
          this.preselected++
        } else {
          this.firstTouch = false
        }
        let el = document.getElementsByClassName("vbta-suggestion")[this.preselected]
        el.style['background-color'] = '#00d1b2'
        
        if (this.preselected != 0) {
          let prev = document.getElementsByClassName("vbta-suggestion")[this.preselected - 1]
          prev.style['background-color'] = '#ffffff'
        }
      }
    },
    emitSelect (value) {
      value = value.replace(/<[\/]?strong>/gm, '')
      this.selected = true
      this.query = value
      this.onSelect(value, this.name)
    },
    getMatches (query) {
      if (query) {
        let matches = []
        let regex = new RegExp(query, 'i')
        let isMatch = false
        this.source.forEach(value => {
          if (typeof value !== 'string') new TypeError(`Typeahead sources must be string. Received ${typeof value}.`)
          if (matches.length === this.limit) return

          let regexProps = regex.exec(value)
          if (regexProps) {
            isMatch = true

            let substr1 = value.substring(0, regexProps.index)
            let substr2 = `<strong>${value.slice(regexProps.index, regexProps.index + query.length)}</strong>`
            let substr3 = value.substring(regexProps.index + query.length)

            let match = substr1 + substr2 + substr3

            matches.push(match)

            if (regexProps.index == 0) {
              let hint = matches[0].replace(/<[\/]?strong>/gm, '').substring(query.length)
              if (hint !== this.hint) this.hint = query + hint
            }
          }
          if (!isMatch) {
            this.hint = ''
          }
        })
        this.matches = matches
      } else {
        this.hint = ''
        this.matches = []
      }
    }
  }
}
</script>

<style lang="scss" scoped>
@import "~bulma/sass/utilities/_all";
@import "~bulma/sass/elements/form.sass";

.vbta {
  width: 100%;
  position: relative;
  display: inline-block;
}

.vbta-input {
  background-color: transparent;
}

.vbta-menu.visible {
  display: inline-block;
}

.vbta-menu {
  position: absolute;
  width: 100%;
  top: 100%;
  left: 0;
  z-index: 1000;
  display: none;
  float: left;
  min-width: 160px;
  padding: 5px 0;
  margin: 2px 0 0;
  list-style: none;
  font-size: 14px;
  text-align: left;
  background-color: #ffffff;
  border: 1px solid #cccccc;
  border: 1px solid rgba(0, 0, 0, 0.15);
  border-radius: 4px;
  -webkit-box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175);
  background-clip: padding-box;
}

.vbta-suggestion {
  display: block;
  padding: 3px 20px;
  clear: both;
  font-weight: normal;
  line-height: 1.42857143;
  color: #333333;
  white-space: nowrap;
}

.vbta-suggestion:hover,
.vbta-suggestion:focus {
  color: #ffffff;
  text-decoration: none;
  outline: 0;
  background-color: #00d1b2;
  cursor: pointer;
}

.vbta-hint {
  color: #999;
  position: absolute;
  top: 0px;
  left: 0px;
  border-color: transparent;
  box-shadow: none;
  opacity: 1;
  background: none 0% 0% / auto repeat scroll padding-box border-box rgb(255, 255, 255);
}
</style>
