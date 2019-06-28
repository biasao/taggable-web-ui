<template>
  <mdb-container>
    <mdb-row>
      <mdb-col col="9">
        <h2 class="text-uppercase my-3">Your feed:</h2>
        <Event
          v-for="(event, index) in events"
          :index="index"
          :url="event.url"
          :title="event.title"
          :tags="event.tags"
          :key="index"
          @delete="handleDelete"
        />
        <mdb-row>
          <mdb-col xl="3" md="6" class="mx-auto text-center">
            <mdb-btn color="info" @click.native="modal = true">Start tagging</mdb-btn>
          </mdb-col>
        </mdb-row>
      </mdb-col>
      <mdb-col col="3">        
        <h1 class="my-3">
          <mdb-row>
            <mdb-col col="7">Taggable</mdb-col>
            <mdb-col col="3" class="text-center">
              <mdb-icon icon="tag"/>
            </mdb-col>
          </mdb-row>
          <h6 class="my-3">
          You have
          <b>{{events.length}} tags</b> today.
        </h6>
        </h1>
      </mdb-col>
    </mdb-row>

    <mdb-modal v-if="modal" @close="modal = false">
      <mdb-modal-header>
        <mdb-modal-title tag="h4" class="w-100 text-center font-weight-bold">Start tagging</mdb-modal-title>
      </mdb-modal-header>
      <mdb-modal-body>
        <form class="mx-3 grey-text">
          <mdb-input
            name="share"
            label="What do you want to share"
            icon="edit"
            type="text"
            @input="handleInput($event, 'title')"
          />
          <mdb-input
            name="tag"
            label="Tag your friends"
            icon="share"
            type="text"
            @input="handleInput($event, 'tags')"
          />
        </form>
      </mdb-modal-body>
      <mdb-modal-footer class="justify-content-center">
        <mdb-btn color="info" @click.native="addEvent">Tag</mdb-btn>
      </mdb-modal-footer>
    </mdb-modal>
  </mdb-container>
</template>

<script>
import {
  mdbContainer,
  mdbRow,
  mdbCol,
  mdbIcon,
  mdbBtn,
  mdbModal,
  mdbModalHeader,
  mdbModalTitle,
  mdbModalBody,
  mdbModalFooter,
  mdbInput,
  mdbTextarea
} from "mdbvue";
import Event from "@/components/Event";
export default {
  name: "App",
  components: {
    mdbContainer,
    mdbRow,
    mdbCol,
    mdbIcon,
    mdbBtn,
    mdbModal,
    mdbModalHeader,
    mdbModalTitle,
    mdbModalBody,
    mdbModalFooter,
    mdbInput,
    mdbTextarea,
    Event
  },
  data() {
    return {
      events: [
        {
          url: "https://medium.com/swlh/why-bootstrapping-your-startup-is-worth-the-struggle-527a04e67d6d",
          title: "https://medium.com/swlh/why-bootstrapping-your-startup-is-worth-the-struggle-527a04e67d6d",
          tags: "@biasao @mirian"
        },
        {
          url: "https://i.imgur.com/lmn1jqO.jpg",
          title: "https://i.imgur.com/lmn1jqO.jpg",
          tags: "@biasao @mateus"
        },
        {
          url: "trello.com",
          title: "trello.com",
          tags: "@biasao @miguel",
        }
      ],
      modal: false,
      newValues: []
    };
  },
  methods: {
    handleDelete(eventIndex) {
      this.events.splice(eventIndex, 1);
    },
    handleInput(val, type) {
      this.newValues[type] = val;
    },
    addEvent() {
      this.events.push({
        url: this.newValues["url"],
        title: this.newValues["title"],
        tags: this.newValues["tags"]
      });
    }
  }
};
</script>

<style>
</style>