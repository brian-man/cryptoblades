<template>
  <b-navbar class="main-nav" toggleable="sm" type="dark" variant="dark">
    <b-navbar-brand href="#">
      <img src="../assets/logo.png" class="logo d-inline-block align-top" alt="Logo">
    </b-navbar-brand>

    <view-links></view-links>

    <skill-balance-display class="ml-auto d-none d-sm-flex" />

    <claim-rewards  v-if="!canShowRewardsBar" />

    <options class="d-none d-sm-flex"/>

    <!-- Render only on mobile view -->
    <div class="d-flex d-sm-none">
      <skill-balance-display class="skill-display-mobile" />
      <options class="options-display-mobile"/>
    </div>
  </b-navbar>
</template>

<script lang="ts">
import Vue from 'vue';

import ViewLinks from './ViewLinks.vue';
import Options from './Options.vue';
import SkillBalanceDisplay from './smart/SkillBalanceDisplay.vue';
import ClaimRewards from './smart/ClaimRewards.vue';

import Events from '../events';

export default Vue.extend({
  components: {
    ViewLinks,
    SkillBalanceDisplay,
    ClaimRewards,
    Options
  },


  data() {
    return {
      canShowRewardsBar: true
    };
  },

  methods: {
    checkStorage(): void {
      this.canShowRewardsBar = !localStorage.getItem('rewards');
    }
  },

  mounted() {
    this.checkStorage();

    Events.$on('setting:rewards', () => this.checkStorage());
  },
});
</script>

<style>

/** Suggest to move this to atomic folder structure like assets/css **/
a {
  text-decoration: none;
  user-select: none;
  color: #b3b0a7 !important;
}

a:hover,
a.router-link-active {
  color: #f2e3bc;
  text-shadow: 0 0 5px #333, 0 0 10px #333, 0 0 15px #e1bb34, 0 0 10px #e1bb34;
  text-decoration: none !important;
}

.dropdown-menu {
  background: rgb(20,20,20);
  background: linear-gradient(45deg, rgba(20,20,20,1) 0%, rgba(36,39,32,1) 100%);
  border: none !important;
}

.dropdown-menu li a:hover {
  background: transparent !important;
}

@media (max-width: 576px) {
  .main-nav {
    align-items: normal !important; /** force only for mobile to manually set alignments **/
    flex-direction: column;
  }
  .main-nav > .navbar-brand {
    align-self: center;
  }
  .main-nav > .navbar-nav {
    flex-direction: row;
    justify-content: space-evenly;
  }
  .skill-display-mobile  {
    flex: 5;
  }
  .options-display-mobile {
    flex: 1;
    align-items: flex-end;
  }
}
</style>

<style scoped>
.logo {
  max-width: 130px;
  padding-top: 7px;
}

.navbar {
  background: rgb(20,20,20);
  background: linear-gradient(45deg, rgba(20,20,20,1) 0%, rgba(36,39,32,1) 100%);
}
</style>


