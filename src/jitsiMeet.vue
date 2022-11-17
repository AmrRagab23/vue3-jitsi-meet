<template>
  <div
    id="jitsiContainer"
    style="overflow: hidden; height: 100%; width: 100%"
    height="100%"
    width="100%"
  ></div>
</template>
<script lang="ts">
import { defineComponent, onMounted } from "vue";
import { loadScript } from "vue-plugin-load-script";
export default defineComponent({
  name: "jitsiComponent",
props :{
  options : Object
},
  setup() {
    let JitsiMeetExternalAPI;
    onMounted(async () => {
      loadScript("https://meet.jit.si/external_api.js")
        .then(() => {
          JitsiMeetExternalAPI = window.JitsiMeetExternalAPI;
          LoadPage();
          console.log("jquery AND jit Is Loaded");
        })
        .catch((ex : any) => {
          console.log("Failed to fetch script", ex);
        });
    });
 
    let api;
    const configOverwrite = {
      startSilent: true,
      enableWelcomePage: false,
      startWithAudioMuted: true,
      disableAddingBackgroundImages: true,
    };
    async function testEv() {
      api.executeCommand("hangup");
    }
    async function LoadPage() {
      const options = {
        roomName: "محاضرة عربى",
        width: "100%",
        height: window.innerHeight,
        parentNode: document.querySelector("#jitsiContainer"),
        lang: "ar",
        userInfo: {
          email: "email@jitsiexamplemail.com",
          displayName: "John Doe",
        },
        configOverwrite: {
          startWithAudioMuted: true,
          startSilent: true,
          enableWelcomePage: false,
          disableAddingBackgroundImages: true,
        },
        interfaceConfigOverwrite: { DISABLE_DOMINANT_SPEAKER_INDICATOR: true },
      };
      api = new JitsiMeetExternalAPI("meet.jit.si", options);

      console.log("LoadPage api", api);
      api.addListener("videoConferenceJoined", onConnectionSuccess);
      api.addListener("videoConferenceLeft", videoConferenceLeft);
    }

    return { testEv };
  },
});
</script>
