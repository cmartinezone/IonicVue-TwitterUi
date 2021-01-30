<template>
  <ion-row class="wrapper">
    <ion-col size="2">
      <ion-avatar>
        <ion-img :src="tweet.img"></ion-img>
      </ion-avatar>
    </ion-col>
    <ion-col size="10">
      <ion-row class="tweet-info">
        <ion-col size="12">
          <span class="name">{{ tweet.username }}</span>
          <span class="handle">@{{ tweet.handle }}</span>
          <span class="handle">- {{ shortDate(tweet.date) }}</span>
        </ion-col>
      </ion-row>
      <ion-row>
        <ion-col size="12">
          <div v-html="convertMentions(convertHashTags(tweet.text))"></div>
          <img class="preview-img" :src="tweet.attachment" v-if="tweet.attachment" />
        </ion-col>
      </ion-row>
      <ion-row class="ion-justify-content-start">
        <ion-col>
          <ion-button fill="clear" color="medium" size="small">
            <ion-icon :icon="chatbubbleOutline" slot="start"></ion-icon>
            {{ tweet.response }}
          </ion-button>
        </ion-col>
        <ion-col>
          <ion-button
            @click="retweet()"
            fill="clear"
            :color="tweet.retweet ? 'primary' : 'medium'"
            size="small"
          >
            <ion-icon :icon="repeatOutline" slot="start"></ion-icon>
            {{ tweet.retweets }}
          </ion-button>
        </ion-col>
        <ion-col>
          <ion-button
            @click="liked()"
            fill="clear"
            :color="tweet.liked ? 'primary' : 'medium'"
            size="small"
          >
            <ion-icon :icon="heartOutline" slot="start"></ion-icon>
            {{ tweet.like }}
          </ion-button>
        </ion-col>
        <ion-col>
          <ion-button fill="clear" color="medium" size="small">
            <ion-icon :icon="shareOutline" slot="start"></ion-icon>
          </ion-button>
        </ion-col>
      </ion-row>
    </ion-col>
  </ion-row>
</template>

<script>
//import { computed } from "vue";
import { IonRow, IonCol, IonAvatar, IonImg, IonButton, IonIcon } from "@ionic/vue";

import {
  chatbubbleOutline,
  repeatOutline,
  heartOutline,
  shareOutline,
} from "ionicons/icons";

export default {
  name: "Tweet",
  components: {
    IonRow,
    IonCol,
    IonAvatar,
    IonImg,
    IonButton,
    IonIcon,
  },
  props: ["tweet"],
  emits: ["like", "retweet"],
  setup(props, { emit }) {
    const liked = () => emit("like");
    const retweet = () => emit("retweet");

    const parseTweet = (text) => {
      return text;
    };

    const shortDate = (date) => {
      return new Date(date * 1000).toLocaleDateString();
    };

    const convertHashTags = (str) => {
      return str.replace(/#([\w]+)/g, '<span class="highlight">#$1</span>');
    };

    const convertMentions = (str) => {
      return str.replace(/@([\w]+)/g, '<span class="highlight">@$1</span>');
    };

    return {
      convertHashTags,
      convertMentions,
      liked,
      retweet,
      shortDate,
      parseTweet,
      /* Icons */
      chatbubbleOutline,
      repeatOutline,
      heartOutline,
      shareOutline,
    };
  },
};
</script>

<style>
.tweet-info {
  font-size: 0.9em;
}

.name {
  font-weight: 600;
}

.handle {
  padding-left: 4px;
  color: var(--ion-color-medium);
}

.wrapper {
  border-bottom: 1px solid var(--ion-color-light);
}

.highlight {
  color: var(--ion-color-primary);
}

.preview-img {
  border: 1px solid var(--ion-color-light);
  border-radius: 10px;
}
</style>
