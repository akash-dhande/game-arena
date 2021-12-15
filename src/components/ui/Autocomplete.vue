<template>
  <div class="form-control autocomplete">
    <input
      v-model="search"
      placeholder="Search Game by Name"
      @focus="showAutocomplete = true"
      @blur="hideAutocomplete"
      @keyup="(e) => updateSearch(e.target.value)"
      class="form-control"
      type="search"
    />
    <ul v-if="showAutocomplete" class="autocomplete-list">
      <li v-for="option of filteredOptions" @click="updateSearch(option.title)">
        {{ option.title }}
      </li>
      <li v-if="filteredOptions.length < 1">No result found</li>
    </ul>
  </div>
</template>
<script>
export default {
  data() {
    return {
      search: '',
      showAutocomplete: false,
    };
  },
  props: {
    onChange: {
      type: Function,
      default: () => {},
    },
    options: {
      type: Array,
      default: () => [],
    },
  },
  computed: {
    filteredOptions() {
      let term = this.search ? this.search.toLowerCase() : '';
      return this.options.filter((item) =>
        item.title.toLowerCase().includes(term)
      );
    },
  },
  methods: {
    updateSearch(val) {
      let timer;
      timer = setTimeout(() => {
        clearTimeout(timer);
        this.onChange(val);
      }, 100);
    },
    hideAutocomplete() {
      setTimeout(() => {
        this.showAutocomplete = false;
      }, 300);
    },
  },
};
</script>
