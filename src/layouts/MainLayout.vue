<template>
  <QLayout view="lHh Lpr lFf">
    <QHeader>
      <QToolbar>
        <QBtn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />
      </QToolbar>

      <div class="q-px-lg q-pt-xl q-mb-md">
        <div class="text-h3">Todo</div>
        <div class="text-subtitle1">{{ todayDate }}</div>
      </div>

      <QImg
        src="~/assets/background.jpg"
        class="header-image full-height absolute-top"
      />
    </QHeader>

    <QDrawer
      v-model="leftDrawerOpen"
      :width="250"
      :breakpoint="600"
      show-if-above
    >
      <QScrollArea
        style="
          height: calc(100% - 192px);
          margin-top: 192px;
          border-right: 1px solid #ddd;
        "
      >
        <QList padding>
          <QItem to="/" exact clickable v-ripple>
            <QItemSection avatar>
              <QIcon name="list" />
            </QItemSection>

            <QItemSection> Todo </QItemSection>
          </QItem>

          <QItem :to="{ name: 'help' }" exact clickable v-ripple>
            <QItemSection avatar>
              <QIcon name="help" />
            </QItemSection>

            <QInterSection> Help </QInterSection>
          </QItem>
        </QList>
      </QScrollArea>

      <QImg
        class="absolute-top"
        src="~/assets/background.jpg"
        style="height: 192px"
      >
        <div class="absolute-bottom bg-transparent">
          <QAvatar class="q-mb-sm" size="56px">
            <img
              src="~/assets/image.jpg"
              style="
                height: 56px;
                width: 56px;
                object-fit: cover;
                border-radius: 50%;
              "
            />
          </QAvatar>

          <div class="text-weight-bold">Adilia Uamusse</div>

          <div>@adiliauamusse</div>
        </div>
      </QImg>
    </QDrawer>

    <QPageContainer>
      <RouterView v-slot="{ Component }">
        <KeepAlive>
          <component :is="Component" />
        </KeepAlive>
      </RouterView>
    </QPageContainer>
  </QLayout>
</template>

<script setup>
import {
  QBtn,
  QDrawer,
  QPageContainer,
  QHeader,
  QIcon,
  QImg,
  QItem,
  QItemSection,
  QScrollArea,
  QToolbar,
  QLayout,
  date,
} from "quasar";
import { ref, computed } from "vue";

const toggleLeftDrawer = () => {
  leftDrawerOpen.value = !leftDrawerOpen.value;
};
const leftDrawerOpen = ref(false);

const todayDate = computed(() => {
  const formattedString = date.formatDate(Date.now(), "dddd D MMMM");
  return formattedString;
});
</script>

<style lang="scss">
.header-image {
  height: 100%;
  z-index: -1;
  opacity: 0.2;
  filter: grayscale(100%);
}
</style>
