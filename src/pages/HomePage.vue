<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-12 text-center">
        <h1>lofi girl clicker - beats to click/relax to</h1>
      </div>
      <!-- Workforce/Purchase Count -->
      <div class="col-md-3">
        <!-- Workspace -->
        <div class="mb-5">
          <h2 class="text-center p-2">
            Workspace
          </h2>
          <div>
            <h5 v-for="upgrade in state.upgrades" :key="upgrade.id">
              {{ upgrade.name }} - {{ upgrade.count }}
            </h5>
          </div>
        </div>
        <!-- Workforce -->
        <div>
          <h2 class="text-center p-2">
            Workforce
          </h2>
          <div>
            <h5 v-for="autoUpgrade in state.autoUpgrades" :key="autoUpgrade.id">
              {{ autoUpgrade.name }} - {{ autoUpgrade.count }}
            </h5>
          </div>
        </div>
      </div>
      <!-- Beats count, click image, bps -->
      <div class="col-md-6">
        <!-- Count Text -->
        <div class="text-center p-2">
          <h2>lofi beats: {{ state.beats }}</h2>
          <h3 class="lead">
            beats per click: {{ state.click }}
          </h3>
          <h3 class="lead">
            beats per second: {{ state.autoClick }}
          </h3>
        </div>
        <!-- Lofi Girl Image -->
        <div class="d-flex justify-content-center">
          <img
            class="click-img"
            src="../assets/img/lofigirl.gif"
            alt="lofi girl"
            @click="state.beats += state.click"
          />
        </div>
      </div>
      <!-- Store -->
      <div class="col-md-3">
        <h2 class="text-center p-2">
          Store
        </h2>
        <div class="row justify-content-around side-bar-scroll">
          <!-- click upgrades -->
          <div class="my-3 text-center">
            <h4>Click Upgrades</h4>
            <div class="m-1" v-for="upgrade in state.upgrades" :key="upgrade.id">
              <button :disabled="upgrade.cost > state.beats" class="btn btn-primary btn-block" type="button" @click="buy(upgrade)">
                <h5 class="m-0">
                  {{ upgrade.name }} <small>🎵 {{ upgrade.cost }}</small>
                </h5>
                <p class="m-0">
                  Bonus: +{{ upgrade.bonus }}
                </p>
              </button>
            </div>
          </div>
          <div class="my-3 text-center">
            <h4>Auto Click Upgrades</h4>
            <div class="m-1" v-for="autoUpgrade in state.autoUpgrades" :key="autoUpgrade.id">
              <button :disabled="autoUpgrade.cost > state.beats" class="btn btn-primary btn-block" type="button" @click="buyAuto(autoUpgrade)">
                <h5 class="m-0">
                  {{ autoUpgrade.name }} <small>🎵 {{ autoUpgrade.cost }}</small>
                </h5>
                <p class="m-0">
                  Bonus: +{{ autoUpgrade.bonus }}
                </p>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive } from '@vue/reactivity'
export default {
  name: 'Home',
  setup() {
    const state = reactive({
      click: 1,
      beats: 0,
      autoClick: 0,
      autoUpgrades: [{
        id: 1,
        name: 'Magic Pen',
        cost: 15,
        bonus: 10,
        count: 0
      }, {
        id: 2,
        name: 'Assistant',
        cost: 100,
        bonus: 50,
        count: 0
      }, {
        id: 3,
        name: 'Ghost Writer',
        cost: 1000,
        bonus: 100,
        count: 0
      }],
      upgrades: [
        {
          id: 1,
          name: 'Coffee',
          cost: 15,
          bonus: 1,
          count: 0
        },
        {
          id: 2,
          name: 'New Headphones',
          cost: 50,
          bonus: 5,
          count: 0
        }, {
          id: 3,
          name: 'Comfier Chair',
          cost: 100,
          bonus: 10,
          count: 0
        }
      ]
    })
    return {
      state,
      buy(upgrade) {
        state.beats -= upgrade.cost
        upgrade.cost = upgrade.cost * 2
        state.click += upgrade.bonus
        upgrade.count++
      },
      buyAuto(autoUpgrade) {
        state.beats -= autoUpgrade.cost
        autoUpgrade.cost = autoUpgrade.cost * 2
        state.autoClick += autoUpgrade.bonus
        autoUpgrade.count++
      },
      startInterval() {
        setInterval(this.autoClick, 1000)
      },
      autoClick() {
        for (let i = 0; i < state.autoUpgrade.length; i++) {
          state.beats += i.bonus * i.count
        }
      }
    }
  }
}
</script>

<style scoped lang="scss">
</style>
