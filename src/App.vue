<template>
  <mdb-container>
    <mdb-row>
      <mdb-col col="9">
        <h2 class="text-uppercase my-3">Your feed:</h2>
        <Tag
          v-for="(tag, index) in tags"
          :index="index"
          :url="tag.url"
          :targetUsers="tag.targetUsers"
          :sourceUser="tag.sourceUser"
          :key="index"
          @delete="handleDelete"
        />
        <g-signin-button
          :params="googleSignInParams"
          @success="onSignInSuccess"
          @error="onSignInError">
          Sign in with Google
        </g-signin-button>
        <mdb-btn color="info" @click.native="getTags">getTags</mdb-btn>
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
          <b>{{tags.length}} tags</b> today.
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
            @input="handleInput($tag, 'title')"
          />
          <mdb-input
            name="tag"
            label="Tag your friends"
            icon="share"
            type="text"
            @input="handleInput($tag, 'tags')"
          />
        </form>
      </mdb-modal-body>
      <mdb-modal-footer class="justify-content-center">
        <mdb-btn color="info" @click.native="addTag">Tag</mdb-btn>
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
import Tag from "@/components/Tag";
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
    Tag
  },
  data() {
    return {
      tags: [
        {
          "id": "",
          "url": "",
          "sourceUser": {
            "handle": "",
            "name": ""
          },
          "targetUsers": [
            {
              "handle": "",
              "name": ""
            },
            {
              "handle": "",
              "name": ""
            }
          ]
        }
      ],
      googleSignInParams: {
        client_id: '955456615163-7hcgcmd9tr074e0b291ue4hssdkcqo9n.apps.googleusercontent.com'
      },
      modal: false,
      newValues: [],
      user: ""
    };
  },
  methods: {
    handleDelete(tagIndex) {
      this.tags.splice(tagIndex, 1);
    },
    handleInput(val, type) {
      this.newValues[type] = val;
    },
    addTag() {
      this.tags.push({
        url: this.newValues["url"],
        targetUsers: this.newValues["targetUsers"]
      });
    },
    getTags: function(){
        if(this.user) {
          var usernameAsHandle = this.user.replace(/@.*$/,"") // TODO: remove this, and let user select handle while registering
          console.log(`get tags for ${this.user}`)

          this.$http.get(`http://6d922be8.ngrok.io/feeds/${usernameAsHandle}`).then(function(response){
              this.tags = response.data.tags;
          }, function(error){
              console.log(error.statusText);
          });
        }
    },
    onSignInSuccess (googleUser) {
      // `googleUser` is the GoogleUser object that represents the just-signed-in user.
      // See https://developers.google.com/identity/sign-in/web/reference#users
      const profile = googleUser.getBasicProfile()
      this.user = profile.U3
      console.log(this.user)
    },
    onSignInError (error) {
      // `error` contains any error occurred.
      console.log('OH NOES', error)
    }
  },
  mounted: function () {
    this.getTags();
  }
};
</script>

<style>
</style>