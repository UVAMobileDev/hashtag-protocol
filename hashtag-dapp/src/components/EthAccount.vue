<template>
  <span>
    <b-tooltip :label="value" position="is-bottom" type="is-dark">
      <span v-if="route">
        <router-link :to="{ name: route, params: { address: value } }">
          <span v-if="ens">
            {{ ens }}
          </span>
          <span v-else>
            {{ value | shortEth }}
          </span>
        </router-link>
      </span>
      <span v-else>
        <span v-if="ens">
          {{ ens }}
        </span>
        <span v-else>
          {{ value | shortEth }}
        </span>
      </span>
    </b-tooltip>
    &nbsp;&nbsp;
    <b-tooltip
      label="view on Etherscan"
      position="is-bottom"
      type="is-dark"
      size="is-small"
    >
      <a :href="this.addressUrl" target="_blank">
        <b-icon icon="ethereum" type="is-grey-light" size="is-small"> </b-icon>
      </a>
    </b-tooltip>
  </span>
</template>

<script>
import { mapGetters } from "vuex";

export default {
  name: "EthAccount",
  props: {
    value: String,
    route: String,
  },
  computed: mapGetters(["homesteadProvider"]),
  data() {
    return {
      ens: null,
      addressUrl: "",
    };
  },
  async mounted() {
    this.ens = this.homesteadProvider
      ? await this.homesteadProvider.lookupAddress(this.value)
      : null;
    this.addressUrl = `${this.etherscanBaseUrl}/address/${this.value}`;
  },
};
</script>
