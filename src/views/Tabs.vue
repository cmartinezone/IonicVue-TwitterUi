<template>
  <ion-page>
    <ion-tabs @ionTabsDidChange="setSelectedTab($event)">
      <ion-tab-bar slot="bottom">
        <ion-tab-button tab="tab1" href="/tabs/tab1">
          <ion-icon :icon="selectedTab === 'tab1' ? home : homeOutline" />
        </ion-tab-button>

        <ion-tab-button tab="tab2" href="/tabs/tab2">
          <ion-icon :icon="selectedTab === 'tab2' ? search : searchOutline" />
        </ion-tab-button>

        <ion-tab-button tab="tab3" href="/tabs/tab3">
          <ion-badge color="dark">11</ion-badge>
          <ion-icon
            :icon="selectedTab === 'tab3' ? notifications : notificationsOutline"
          />
        </ion-tab-button>

        <ion-tab-button tab="tab4" href="/tabs/tab4">
          <ion-icon :icon="selectedTab === 'tab4' ? mail : mailOutline" />
        </ion-tab-button>
      </ion-tab-bar>
    </ion-tabs>
  </ion-page>
</template>

<script>
import { ref } from "vue";
import { IonTabBar, IonTabButton, IonTabs, IonIcon, IonPage, IonBadge } from "@ionic/vue";
import {
  homeOutline,
  home,
  search,
  searchOutline,
  notifications,
  notificationsOutline,
  mail,
  mailOutline,
} from "ionicons/icons";

export default {
  name: "Tabs",
  components: { IonTabs, IonTabBar, IonTabButton, IonIcon, IonPage, IonBadge },
  setup() {
    let selectedTab = ref();

    const setSelectedTab = (event) => {
      selectedTab.value = event.tab;
    };

    return {
      selectedTab,
      setSelectedTab,
      /* Icons */
      homeOutline,
      home,
      search,
      searchOutline,
      notifications,
      notificationsOutline,
      mail,
      mailOutline,
    };
  },
};
</script>
<style scoped>
ion-badge {
  top: 7px;
  left: calc(50% + 1px);
  font-size: 9px;
  font-weight: 500;
  border-radius: 10px;
  padding: 2px 2px 2px;
  min-width: 18px;
  min-height: 18px;
  line-height: 14px;
}
</style>
