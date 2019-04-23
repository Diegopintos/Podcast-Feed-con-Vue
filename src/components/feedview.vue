<template>
  <div class="uk-background-primary uk-height-viewport">
    

      <ul class="uk-switcher uk-margin my-podcasts">
          <li>
            <table class="uk-table uk-table-middle uk-table-divider uk-table-hover uk-background-primary">
                <thead style="display: table-footer-group;">
                    <tr>
                        <th class="uk-table-auto uk-text-center"></th>
                        <th class="uk-table-shrink uk-text-center"></th>
                    
                    </tr>
                </thead>
                <tbody>
                  <div v-if="!feed.title" class="uk-position-center uk-text-center"><img src="src/img/podcasts.svg" width="300px"></div>
                    <tr v-for="entry in feed.entries" @click="play(entry.enclosure.url)">
                        <td class="uk-text-truncate uk-light uk-text-bold">
                          <span v-if="selectedTrack == entry.enclosure.url" class="pod_active pull-left padding-right-10"></span>
                          <span class="uk-margin-small-right"  uk-icon="rss" ratio="1"></span> <span>{{ getShortTitle(entry.title) }}</span>
                        </td>
                        <td><span class="uk-label uk-label-success">{{ entry.itunes.duration }}</span></td>
                    </tr>
                </tbody>
            </table>
          </li>
          <li>
              <div v-if="!feed.title" class="uk-text-center">Escolle Podcast</div>
                <div class="uk-child-width-expand@s uk-grid-collapse uk-text-center" uk-grid matched v-else>
                     <div class="uk-background-cover uk-height-viewport uk-flex uk-flex-center uk-flex-middle uk-light" style="background-image: url(//picsum.photos/600/600/?random);">
                          <div class="uk-h4 uk-overlay-primary uk-padding-small uk-text-left uk-width-1-2@m">
                           <p> {{ feed.description || '' }}</p>
                             <div class="link-feed uk-text-right"><a :href="feed.link" target="_blank"><span uk-icon="rss" ratio="1.2"></span></a></div>
                
                          </div>
                          
                      </div>
                    </div>
          </li>
      
      </ul>
  </div>
</template>

<script>
export default {
  name: 'feedview',

  props: {
    feed: {
      type: Object,
      default: {},
      required: true
    },

    selectedTrack: {
      type: String,
      default: "",
      required: true
    }
  },

  methods: {
    play (url) {
      this.$emit("on-track-select", url)
    },

    getShortTitle(title) {
      if (!title) return "(No Title)"
      if (title.length > 90) return `${title.substr(0, 90)} ....`
      else return title
    }
  }
}
</script>

<style scope>

.uk-table-hover tbody tr:hover, .uk-table-hover>tr:hover {
    background: #383838;
}

.link-feed a {
    background: azure;
    color: black;
    padding: 5px 5px 10px;
}
span.pod_active {
    padding: 0px 10px;
    margin-right: 10px;
    background: #F44336;
    border: 1px solid #0000004f;
    border-radius: 100%;
}
  .margin-top-30 {
    margin-top: 30px;
  }

  .padding-right-10 {
    padding-right: 10px;
  }

  .entry-list {
    border: 1px solid #d4d0d0;
    max-height: 250px;
    overflow-x: auto;
  }
ul.uk-tab {
    margin-bottom: 0px;
    background: black;
}

ul.tips-cero {
    margin: 0px;
}

ul.tips-cero li {
    margin-right: 15px;
}
</style>