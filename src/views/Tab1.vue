<template>
  <ion-page>
    <ion-header ref="header">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-button>
            <ion-icon slot="icon-only" :icon="menuOutline"></ion-icon>
          </ion-button>
        </ion-buttons>
        <ion-title>
          <ion-icon :icon="logoTwitter" color="primary" size="large"></ion-icon>
        </ion-title>
        <ion-buttons slot="end">
          <ion-button>
            <ion-icon slot="icon-only" :icon="pulseOutline"></ion-icon>
          </ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>

    <ion-segment v-model="segment" mode="md" ref="segmentcontrol">
      <ion-segment-button value="home">
        <ion-label>Home</ion-label>
      </ion-segment-button>
      <ion-segment-button value="content">
        <ion-label>cr-content-suggest</ion-label>
      </ion-segment-button>
    </ion-segment>

    <!-- Segment container  -->
    <ion-content
      v-if="!loading"
      :fullscreen="true"
      :scroll-events="true"
      v-
      @ionScroll="scrollContent($event)"
    >
      <div v-show="segment === 'home'">
        <!-- Stoy section at the top -->
        <ion-slides :options="opts">
          <ion-slide v-for="(tweet, index) in tweets" :key="index">
            <ion-avatar>
              <img :src="tweet.img" />
            </ion-avatar>
          </ion-slide>
        </ion-slides>

        <!-- List of Tweets -->
        <app-tweet
          v-for="(tweet, index) in tweets"
          :key="index"
          :tweet="tweet"
          @like="tweet.liked = !tweet.liked"
          @retweet="tweet.retweet = !tweet.retweet"
        ></app-tweet>
      </div>

      <ion-fab vertical="bottom" horizontal="end" slot="fixed">
        <ion-fab-button>
          <ion-icon :icon="pencil"></ion-icon>
        </ion-fab-button>
      </ion-fab>

      <div v-show="segment === 'content'">Other content</div>
    </ion-content>

    <ion-content v-else>
      <div class="loading-center">
        <ion-spinner color="primary"></ion-spinner>
      </div>
    </ion-content>
  </ion-page>
</template>

<script>
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonSpinner,
  IonIcon,
  IonLabel,
  IonButtons,
  IonButton,
  IonSegment,
  IonSegmentButton,
  IonAvatar,
  IonSlide,
  IonSlides,
  IonFabButton,
  IonFab,
  isPlatform,
} from "@ionic/vue";
//import ExploreContainer from "@/components/ExploreContainer.vue";
import axios from "axios";
import { reactive, toRefs, ref } from "vue";
import { menuOutline, logoTwitter, pulseOutline, pencil } from "ionicons/icons";
import Tweet from "@/components/Tweet.vue";

export default {
  name: "Tab1",
  components: {
    IonHeader,
    IonToolbar,
    IonTitle,
    IonContent,
    IonPage,
    IonSpinner,
    IonIcon,
    IonLabel,
    IonButtons,
    IonButton,
    IonSegment,
    IonSegmentButton,
    IonAvatar,
    IonSlide,
    IonSlides,
    IonFabButton,
    IonFab,
    "app-tweet": Tweet,
  },
  setup() {
    let header = ref();
    let segmentcontrol = ref();
    let headerHeight = ref(isPlatform("ios") ? 44 : 56);

    const state = reactive({
      tweets: {},
      loading: false,
      segment: "home",
      opts: {
        slidesPerView: 4.5,
        spaceBetween: 10,
        slidesOffsetBefore: 0,
      },
    });

    const fetchTweets = async (dispLoaderPage) => {
      if (dispLoaderPage) {
        state.loading = true;
      }

      const res = await axios.get(
        "https://devdactic.fra1.digitaloceanspaces.com/twitter-ui/tweets.json"
      );

      if (res.data) {
        state.tweets = res.data.tweets;
      }

      state.loading = false;
    };

    fetchTweets(true);

    const scrollContent = (event) => {
      const scrollTop = event.detail.scrollTop;
      let newPositon = -scrollTop;

      if (newPositon < -headerHeight.value) {
        newPositon = -headerHeight.value;
      }
      let newOpacity = 1 - newPositon / -headerHeight.value;

      for (let c of header.value.$el.children) {
        c.style.opacity = newOpacity;
      }

      header.value.$el.style.top = newPositon + "px";
      segmentcontrol.value.$el.style.top = newPositon + "px";
    };

    return {
      scrollContent,
      header,
      segmentcontrol,
      headerHeight,

      ...toRefs(state),
      /* Icons */
      menuOutline,
      logoTwitter,
      pulseOutline,
      pencil,
    };
  },
  directives: {
    /* It does not work, it does not listen to the event */
    appHideHeader: {
      mounted(el, binding) {
        console.log(binding.value);
        el.addEventListener("ionScroll", (ev) => console.log("scroll", ev.detail));
      },
    },
  },
};
</script>

<style scoped>
.loading-center {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 90vh;
}

ion-spinner {
  transform: scale(1.5);
}

ion-toolbar {
  --border-style: none;
}

ion-header {
  background: #fff;
}

ion-slides {
  padding-top: 8px;
  padding-bottom: 8px;
  border-bottom: 1px solid var(--ion-color-light);
}

ion-segment {
  z-index: 10;
  background: #fff !important;
}

ion-segment-button {
  text-transform: none;
}

ion-avatar {
  border: 2px solid var(--ion-color-primary);
  padding: 2px;
}
</style>
