<template>
  <div class="home">
    <Frame minBtn maxBtn closeBtn />
    <h2>{{version}}</h2>
  </div>
</template>

<script>
// @ is an alias to /src
import Frame from "@/components/Frame";
const { ipcRenderer } = window.require("electron");
import swal from 'sweetalert'

export default {
  name: "Home",
  components: {
    Frame,
  },
  data() {
    return {
      version: ""
    }
  },
  mounted() {
    ipcRenderer.send('app_version');
    ipcRenderer.on('app_version', (event, arg) => {
      ipcRenderer.removeAllListeners('app_version');
      this.version = 'Version ' + arg.version;
    });

    ipcRenderer.on("update_available", () => {
      ipcRenderer.removeAllListeners("update_available");
      swal("A new update is available. Downloading now...")
    });
    
    ipcRenderer.on("update_downloaded", () => {
      ipcRenderer.removeAllListeners("update_downloaded");
      swal("Update Downloaded. It will be installed on restart. Restart now?")
      .then(restart => {
        if (restart)
        {
          ipcRenderer.send('restart_app');
        }
      })
    });
  },
};
</script>
