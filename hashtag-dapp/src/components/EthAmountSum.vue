<template>
  <span>{{ sum() | toEth | toDp }} Ξ</span>
</template>

<script>
import { ethers } from "ethers";

export default {
  name: "EthAmountSum",
  props: ["value1", "value2"],
  methods: {
    sum() {
      return ethers.utils
        .bigNumberify(this.value1)
        .add(ethers.utils.bigNumberify(this.value2));
    },
  },
};
</script>
