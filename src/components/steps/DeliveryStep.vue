<template>
  <div class="step-wrapper">
    <v-stepper-step :step="step" :complete="complete" :editable="complete" edit-icon="done">
      <div>
        {{ $t('steps.delivery.title') | capitalize }}
        <span v-if="complete">
          &mdash;
          <a>{{ $t('generic.edit') }}</a>
        </span>
      </div>
      <small>{{ $t('steps.delivery.hint') | capitalize }}</small>
    </v-stepper-step>

    <v-stepper-content :step="step">
      <ShipmentFields
        v-for="(shipment, index) in shipments"
        :shipment="shipment"
        :key="shipment.id"
        :count="index+1"
        :total="shipments.length"
      />
      <v-btn
        id="delivery-step-submit"
        color="primary"
        @click="nextStep"
        :block="isMobile"
        :disabled="disabled"
        :loading="buttons.loading_delivery"
      >{{ $t('steps.delivery.button') }}</v-btn>
    </v-stepper-content>

    <div class="step-summary" v-if="complete">
      <ShipmentSummary
        v-for="(shipment, index) in shipments"
        :shipment="shipment"
        :key="shipment.id"
        :count="index+1"
        :total="shipments.length"
        :editable="false"
      />
    </div>
  </div>
</template>

<script>
import { stepMixin } from '@/mixins/stepMixin'
import { mapMultiRowFields } from 'vuex-map-fields'
import { mapState } from 'vuex'
import ShipmentFields from '@/components/fields/ShipmentFields'
import ShipmentSummary from '@/components/summaries/ShipmentSummary'

export default {
  components: {
    ShipmentFields,
    ShipmentSummary
  },
  mixins: [stepMixin],
  computed: {
    disabled () {
      return this.validations.invalid_shipments
    },
    ...mapState(['validations', 'buttons']),
    ...mapMultiRowFields(['order.shipments'])
  }
}
</script>

<style lang="scss" scoped>
.shipment-summary {
  &:last-child {
    margin-bottom: 1.5rem;
  }
}
</style>
