<template>
  <div class="houses container">
    <!-- NOTE Modal ========================= -->
    <!-- <div class="row">
      <div class="col  py-3">
        <h2>Houses</h2>
        <button title="Open Create House Form"
                type="button"
                class="btn btn-outline-success"
                data-toggle="modal"
                data-target="#new-house-form">
          <i class="fas fa-plus" aria-hidden="true"></i>
        </button>
      </div>
    </div> -->
    <!-- NOTE spinner loading on delayed load -->
    <div class="row">
      <div v-if="state.loading">
        <i class="fas fa-spinner fa-spin"></i>
      </div>
      <!-- NOTE houses go here v-for house in houses  ----------------------------->
      <!-- NOTE :key is unique idenfigier for earch house  -->
      <House v-else v-for="house in state.houses" :key="house.id" :house="house" />
    </div>
  </div>
</template>

<script>
import { computed, onMounted, reactive } from 'vue'
import House from '../components/HouseComponent'
import { housesService } from '../services/HousesService'
import { AppState } from '../AppState'

export default {
  name: 'HousesPage',
  // NOTE setup() magic updates DOM (template) on page reload
  // NOTE reactive() makes it watchable.
  // NOTE computed() watches and recomputes houses if changes made

  // NOTE loading is just part of the spinner running if loader is delaayed?
  setup() {
    const state = reactive({
      loading: true,
      houses: computed(() => AppState.houses)
    })

    // NOTE This fires everytime this component is rendered to the Dom
    // similar to how we were using the 'constructor' of the controllers in MVC
    onMounted(async() => {
      try {
        await housesService.getHouses()
        state.loading = false
      } catch (error) {
        console.error(error)
      }
    })
    return {
      state
    }
  },

  components: {
    House
  }
}
</script>

<style lang="scss" scoped>

</style>
