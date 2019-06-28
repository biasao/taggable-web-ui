<template>
  <div>
    <div class="media mt-1">
      <img v-bind:src="'http://api.miniature.io/?url=' + url">
      <div class="media-body mb-3 mb-lg-3">
        <mdb-badge @click.native="onDelete" tag="a" color="danger-color" class="ml-2 float-right">-</mdb-badge>
        <h6 v-if="title" class="mt-0 font-weight-bold">{{title}}</h6>
        <hr class="hr-bold my-2">
        <p v-if="tags" class="font-smaller mb-0">
          <mdb-icon icon="tag"/>
          {{tags}}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import { mdbBadge, mdbIcon } from "mdbvue";

export default {
  name: "Event",
  components: {
    mdbBadge,
    mdbIcon
  },
  props: {
    index: {
      type: Number
    },
    url: {
      type: String
    },
    title: {
      type: String
    },
    tags: {
      type: String
    }
  },
  methods: {
    onDelete() {
      this.$emit("delete", this.index);
    }
  }
};
</script>

<style scoped>
</style>
