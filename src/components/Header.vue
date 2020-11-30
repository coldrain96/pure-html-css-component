<template>
  <header class="header">
    <form
      class="header__form search-panel"
      @submit.prevent="submitForm"
    >
      <div class="search-panel__filters filters">
        <div class="container">
          <div class="filters__filters-group">
            <div class="rounded flex-row full-width">
              <template v-if="isSearchByNameActive">
                <div class="full-width flex-center">
                  <input
                    v-model="searchInputData"
                    class="search-panel__search-input"
                    type="text"
                    required
                    placeholder="Введите название объекта (бизнес-центра, торгового центра, новостройки, логопарка)"
                  >
                </div>
              </template>

              <template v-else>
                <div class="full-width flex-center">
                  <button
                    disabled
                    @click.prevent
                    class="search-panel__general-button search-panel__button--dropdown search-panel__general-button--sm"
                    style="width: 33.33333% !important;"
                  >
                    Москва
                  </button>
                  <button
                    disabled
                    @click.prevent
                    class="search-panel__general-button search-panel__button--dropdown search-panel__general-button--sm"
                    style="width: 33.33333% !important;"
                  >
                    Арендовать
                  </button>
                  <button
                    disabled
                    @click.prevent
                    class="search-panel__general-button search-panel__button--dropdown search-panel__general-button--sm"
                    style="width: 33.33333% !important;"
                  >
                    Офис
                  </button>
                </div>
              </template>
              <button
                type="submit"
                class="search-button--sm search-panel__search-button search-button"
              >
                Найти
              </button>
            </div>

          </div>

          <div class="filters__filters-group search-button--xs">
            <div class="rounded flex-row full-width">
              <button
                type="submit"
                class="search-panel__search-button search-button full-width"
              >
                Найти
              </button>
            </div>
          </div>
        </div>
      </div>

      <div class="search-panel__filters filters">
        <template v-if="isSearchByNameActive === false">
          <div class="container">
            <template v-if="isPriceFilterActive">
              <div class="filters__filters-group">
                <div
                  class="flex-row flex-center full-width rounded nulify-margin-x-xs"
                  style="max-height: 45px; margin-bottom: 5px; margin-right: 5px">
                  <input
                    type="number"
                    placeholder="От"
                    class="filters__input full-width"
                    v-model="priceValueFrom"
                  >
                  <input
                    type="number"
                    placeholder="До"
                    class="filters__input full-width"
                    v-model="priceValueTo"
                  >
                  <div class="full-width">
                    <button
                      @click.prevent
                      class="filters__button-options-group-unit full-width"
                    >
                      {{ footageValueUnit }}
                    </button>
                  </div>

                </div>
                <div class="full-width button-margin" style="margin-bottom: 5px;">
                  <button
                    class="filters__button-add-option"
                    @click.prevent="swapHelperFilters('footage')"
                  >
                    Добавить метраж
                  </button>
                </div>
              </div>
            </template>
            <template v-if="isFootageFilterActive">
              <div class="filters__filters-group">
                <div class="full-width nulify-margin-x-xs" style="margin-bottom: 5px; margin-right: 5px;">
                  <button
                    class="filters__button-add-option"
                    @click.prevent="swapHelperFilters('price')"
                  >
                    Добавить цену
                  </button>
                </div>
                <div class="flex-row flex-center full-width rounded" style="max-height: 45px; margin-bottom: 5px;">
                  <input
                    type="number"
                    placeholder="От"
                    v-model="footageValueFrom"
                    class="filters__input full-width"
                  >
                  <input
                    type="number"
                    placeholder="До"
                    v-model="footageValueTo"
                    class="filters__input full-width"
                  >
                  <div class="full-width">
                    <button
                      @click.prevent
                      class="filters__button-options-group-unit full-width"
                    >
                      {{ priceValueUnit }}
                    </button>
                  </div>

                </div>
              </div>
            </template>
          </div>
        </template>
      </div>

      <div class="search-panel__helper helper"
      >
        <div class="container">
          <div
            class="helper__helper-group-wrapper"
          >
            <div class="helper--item-group">
              <div
                class="helper__item helper__search-type"
                :class="{'helper__search-type--active': isSearchByNameActive}"
              >
                Искать по названию
                <label class="checkbox">
                  <input type="checkbox" @change="invertSearchType">
                  <span class="checkbox-switch"></span>
                </label>
              </div>

              <!--            <div-->
              <!--              class="helper__item helper__search-type"-->
              <!--              :class="{'helper__search-type&#45;&#45;active': isSearchByNameActive}"-->
              <!--            >-->
              <!--              Искать по названию-->
              <!--            </div>-->
            </div>
            <div class="helper--item-group">
              <div class="flex-row">
                <div
                  class="helper__item helper__item--pointed"
                  @click="swapHelperFilters('price')"
                >
                  <div
                    class="helper__filter-status"
                  >
                    <img
                      v-if="isPriceFilterActive"
                      :src="'/img/close.png'"
                    >
                    <img
                      v-else
                      :src="'/img/add.png'"
                    >
                  </div>
                                  Цена
                </div>

                <div
                  class="helper__item helper__item--pointed"
                  style="margin: 0 10px !important;"
                  @click="swapHelperFilters('footage')"
                >
                  <div
                    class="helper__filter-status"
                  >
                    <img
                      v-if="isFootageFilterActive"
                      :src="'/img/close.png'"
                    >
                    <img
                      v-else
                      :src="'/img/add.png'"
                    >
                  </div>
                                  Метраж
                </div>
              </div>

            </div>
          </div>
        </div>
      </div>
    </form>
  </header>
</template>

<script>
  export default {
    data() {
      return {
        isSearchByNameActive: false,
        isFootageFilterActive: false,
        isPriceFilterActive: true,
        city: 'Москва',
        transactionType: 'Арендовать',
        premisesType: 'Офис',
        searchInputData: '',
        priceValueFrom: '',
        priceValueTo: '',
        priceValueUnit: '₽/месяц',
        footageValueFrom: '',
        footageValueTo: '',
        footageValueUnit: 'м²',
      }
    },
    methods: {
      invertSearchType() {
        this.isSearchByNameActive = !this.isSearchByNameActive
      },
      submitForm() {
        const searchInputData = {
          searchObjectName: this.searchInputData
        }
        const searchPanelFormData = {
          city: this.city,
          transactionType: this.transactionType,
          premisesType: this.premisesType,
          price: {
            from: this.priceValueFrom,
            to: this.priceValueTo,
            unit: this.priceValueUnit
          },
          footage: {
            from: this.footageValueFrom,
            to: this.footageValueTo,
            unit: this.footageValueUnit
          }
        }

        if (this.isSearchByNameActive) {
          this.$emit('formDataOutput', searchInputData)
        } else {
          this.$emit('formDataOutput', searchPanelFormData)
        }
      },
      swapHelperFilters(filter) {
        switch (filter.toLowerCase()) {
          case 'price':
            this.isSearchByNameActive = false

            this.isFootageFilterActive = false;
            this.isPriceFilterActive = true;
            break;
          case 'footage':
            this.isSearchByNameActive = false

            this.isPriceFilterActive = false;
            this.isFootageFilterActive = true;
            break;
          default:
            throw new Error(`${filter} argument is not existence.`)
        }
      },
    }
  }
</script>
