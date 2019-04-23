<template>
  <div>
    <div class="uk-position-relative">

      <nav class="uk-navbar-container" uk-navbar="dropbar: true">

          <div class="uk-navbar-left">

              <ul class="uk-navbar-nav uk-logo">
                  <li>
                      <a href="#"><img src="src/img/podcasts.svg" width="50px"></a>
                      <div class="uk-navbar-dropdown">
                            <feedslist :feeds="feeds" :selected-feed-title="selectedFeed.title" @on-feed-select="feedSelected"/>
      
                      </div>
                  </li>
              </ul>

          </div>
          
       
          <div class="uk-navbar-right">

              <ul class="uk-subnav tips-cero" uk-switcher="connect: .my-podcasts">
                  <li><a href="#"><span uk-icon="icon: list; ratio: 1.5"></span></a></li>
                  <li><a href="#"><span uk-icon="icon: info; ratio: 1.5"></span></span></a></li>
              </ul>
          </div>
      </nav>
      
      <div class="uk-navbar-dropbar"></div>
    </div>
  <!--  <vk-navbar-dropbar>
      <vk-navbar>
       <vk-navbar-nav slot="center">{{feed.title}}</vk-navbar-nav> 
        <vk-navbar-logo slot="right"><img src="src/img/radio.svg" style="width: 40px;"></vk-navbar-logo>   
        <vk-navbar-nav>
           <vk-navbar-nav-dropdown title="Podcast">
            <feedslist :feeds="feeds" :selected-feed-title="selectedFeed.title" @on-feed-select="feedSelected"/>
          </vk-navbar-nav-dropdown>
        </vk-navbar-nav>
        <vk-navbar slot="center">{{selectedFeed.title}}</vk-navbar>
      </vk-navbar>
    </vk-navbar-dropbar> -->

    <div class="uk-container uk-padding-remove uk-container-expand">
      <feedview :feed="selectedFeed" :selected-track="selectedTrack" @on-track-select="trackSelected"/>
    </div>
      
    <div class="footer"><player :track="selectedTrack"/></div>
  </div>
</template>

<script>
import { parseURL } from 'rss-parser'

import feedslist from '@/components/feedslist.vue'
import feedview from '@/components/feedview.vue'
import player from '@/components/player.vue'

export default {
  components: { feedslist, feedview, player },

  data() {
    return {
      feeds: {},
      selectedFeed: {},
      selectedTrack: ""
    }
  },

  methods: {
    feedSelected (name) {
      if (!this.feeds[name]) this.selectedFeed = {}
      this.selectedFeed = this.feeds[name]
    },

    trackSelected (url) {
      if (!url) this.selectedTrack = ""
      this.selectedTrack = url
    }
  },
     getShortTitle(title) {
      if (!title) return "(No Title)"
      if (title.length > 90) return `${title.substr(0, 90)} ....`
      else return title
    },

  mounted() {
    fetch("/static/feeds.json")
      .then(res => res.json())
      .then(res => {
        if (res.length) {
          res.forEach(feed => {
            parseURL(feed, (err, data) => {
              if (err) console.error(err)
              this.$set(this.feeds, data.feed.title, data.feed)
            })

          })
        } else {
          alert("No Podcast urls found")
        }
      })

  }
}
</script>