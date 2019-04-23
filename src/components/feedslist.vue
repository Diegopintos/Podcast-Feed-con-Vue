<template>
        <ul class="uk-nav uk-navbar-dropdown-nav uk-text-lead uk-list uk-list-divider" v-if="feeds != {}">
          <li
            v-for="feed in feeds" 
            @click="select(feed.title)" 
            :class="{active: feed.title == selectedFeedTitle}">
           <span class="uk-margin-small-right" uk-icon="heart"></span> {{ getShortTitle(feed.title) }}
          </li>
          
        </ul>
</template>

<script>

export default {
  name: 'feedslist',

  props: {
    feeds: {
      type: Object,
      default: {},
      required: true
    },

    selectedFeedTitle: {
      type: String,
      default: "",
      required: true
    }
  },

  methods: {
    select (name) {
      this.$emit("on-feed-select", name)
    },

    getShortTitle(title) {
      if (!title) return "(No Title)"
      if (title.length > 35) return `${title.substr(0, 35)} ....`
      else return title
    }
  }
}
</script>

<style scope>

.footer {
    position: fixed;
    left: 0px;
    bottom: 0px;
    height: 50px;
    width: 100%;
    background: #f1f3f4;
}

  li {
    cursor: pointer !important;
  }

  .uk-navbar-dropdown {
    width: 100%;
}
</style>