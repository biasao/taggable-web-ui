<template>
  <div>
    <div class="media mt-1">
      <a :href="url"><img v-bind:src="'http://api.miniature.io/?url=' + url"></a>
      <div class="media-body mb-3 mb-lg-3">
        <mdb-badge @click.native="onDelete" tag="a" color="danger-color" class="ml-2 float-right">-</mdb-badge>
        <a v-bind:href="url"><h6 v-if="url" class="mt-0 font-weight-bold">{{url}}</h6></a>
        <hr class="hr-bold my-2">
        <h6 class="my-6"> from: {{ sourceUser.handle }} </h6>
        <p v-if="targetUsers" class="font-smaller mb-0">
          <div v-for="targetUsers in targetUsers">
            <mdb-icon style="padding: 1%" icon="tag"/> {{ targetUsers.handle }}
          </div>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import { mdbBadge, mdbIcon } from "mdbvue";

export default {
  name: "Tag",
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
    targetUsers: {
      type: Array
    },
    sourceUser: {
      type: Object
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
