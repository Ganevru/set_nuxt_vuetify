<template>
  <div>

    <!-- <DevicesTypesBar /> -->
    <p>&nbsp;</p>

    <v-flex xs12 md6>
      <v-text-field label="Search" v-model="text_search" outline></v-text-field>
    </v-flex>

    <!-- Каталог МЫШЕК -->
    <!-- <div v-if="this.$route.name ==='devices-mouse' "> -->

    <!-- радио фильтры -->
    <v-btn-toggle v-model="mouse_form">
      <v-btn large value="ergonomic">ergonomic</v-btn>
      <v-btn large value="ambidextrous">ambidextrous</v-btn>
    </v-btn-toggle>
    &nbsp;
    <v-btn-toggle v-model="mouse_sensor">
      <v-btn large value="optical">optical</v-btn>
      <v-btn large value="laser">laser</v-btn>
    </v-btn-toggle>

    <p>&nbsp;</p>

    <!-- сортировка -->
    <!-- TODO - сделать компонент для сортировки что бы не повторять в каждом разделе девайсов -->
    <v-flex>
      <v-toolbar dense>
        <v-btn-toggle v-model="devices_sort" class="transparent" mandatory>
          Sort by:&nbsp;
          <v-btn value="title" flat>names</v-btn>
          <v-btn value="ASIN" flat>ASIN</v-btn>
        </v-btn-toggle>
      </v-toolbar>
    </v-flex>

    <transition-group name="fade" tag="section">
      <div v-for="device in SortedDevices" :key="device.ASIN">
        <!-- <div v-if="device.devices === 'mouse'"> -->
        <DeviceCard v-bind:device="device" />
        <p>
          <nuxt-link :to="{ path: `/devices/${device.ASIN}` }">{{ device.ASIN }}</nuxt-link>
        </p>
        <p>
          <nuxt-link :to="{ path: `/devices/${device.device_type}/${device.title}` }">{{device.device_type}}/{{ device.title }}</nuxt-link>
        </p>
        <p>
          <nuxt-link :to="{ path: `/devices/${device.device_type}/${device.ASIN}` }">{{device.device_type}}/{{ device.ASIN }}</nuxt-link>
        </p>
        <!-- </div> -->
      </div>
    </transition-group>

    <!-- </div> -->
    <!-- /Каталог МЫШЕК -->

  </div>
</template>

<script>
import DeviceCard from "~/components/DeviceCard.vue";
import DevicesTypesBar from "~/components/DevicesTypesBar.vue";

export default {
  data: function() {
    return {
      text_search: null,
      // фильтры
      mouse_form: null,
      mouse_sensor: null,
      // сортировка
      devices_sort: "title"
    };
  },
  components: {
    DeviceCard,
    DevicesTypesBar
  },
  methods: {
    FilteredRadioFun: function(ValueOfRadio, ValueOfString) {
      return ValueOfRadio !== null && ValueOfRadio !== ValueOfString;
    }
  },
  computed: {
    // использую vuex
    // первый devices это devices.js в папке store, а второй - devices заданный внутри devices.js
    devices() {
      return this.$store.state.devices.devices;
    },
    // возвращает отфильтрованные фильтрами юзера девайсы
    FilteredDevices: function() {
      let devices = this.devices.filter(device => {
        let filter_matches = true;

        // из https://codepen.io/andreasremdt/pen/rvQJYR?editors=1010
        // freeCodeCamp Twitch Viewer

        // filter_matches определяет для каждого девайса по отдельности
        // пройдет ли он в лист FilteredDevices или нет
        // каждый следующий if это фильтр,
        // если девайс его не проходит то filter_matches = false

        // фильтр по поиску
        if (
          this.text_search !== null &&
          !device.title.toLowerCase().includes(this.text_search.toLowerCase())
        ) {
          filter_matches = false;
        }

        // фильтр по мышиным фильтрам-радио кнопкам
        if (this.FilteredRadioFun(this.mouse_form, device.mouse_form)) {
          filter_matches = false;
        }
        if (this.FilteredRadioFun(this.mouse_sensor, device.mouse_sensor)) {
          filter_matches = false;
        }

        if (filter_matches) return device;
      });

      return devices;
    },
    // возвращает отсортированные сортировкой девайсы
    SortedDevices: function() {
      // https://lodash.com/docs/4.17.10#orderBy
      // https://vuejs.org/v2/guide/migration.html#Replacing-the-orderBy-Filter
      return _.orderBy(this.FilteredDevices, this.devices_sort);
    }
  }
};
</script>

<style scoped>
</style>