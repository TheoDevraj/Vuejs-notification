<template>
<div id="app">
  <h2>Web Notification API Demo</h2>

  <div class="permission-request" v-if="notificationPermissionRequired">
    <div class="permission-request--message">
      Howdy, partner! Let's <span class="underline-link" v-on:click="requestNotificationPermission">enable desktop notifications</span>! &nbsp; &nbsp; &nbsp;
      <span class="dismiss-notification" v-on:click="showPermissionRequest = false">✖</span>
    </div>
  </div>

  <ul class="notification-buttons">
    <li class="button" v-on:click="triggerBasicNotification">
      Basic Notification
    </li>

    <li class="button" v-on:click="triggerNotificationWithIcon">
      Notification with icon
    </li>

    <li class="button" v-on:click="triggerDelayedNotification">
      Delayed Notification
    </li>

    <li class="button" v-on:click="triggerFocusLinkNotification">
      Focus Link Notification
    </li>

    <li class="button" v-on:click="triggerBlurOnlyNotification">
      Blur Only Notification
    </li>

    <li class="button" v-on:click="triggerClearOnClick">
      Clear on Click
    </li>
  </ul>

</div>
</template>

<script>
export default {
  name: 'app',

  created() {},

  data: function() {
    return {
      showPermissionRequest: true
    }
  },

  computed: {
    notificationPermissionRequired: function() {
      // If the notification permission is 'default', or not explicitly
      // granted or denied and our permission request can be shown, return true
      const permission = Notification.permission
      return permission === 'default' && this.showPermissionRequest ? true : false
    }
  },

  methods: {
    requestNotificationPermission: function() {
      this.showPermissionRequest = true

      Notification.requestPermission((permission) => {
        if (permission === "granted") {
          this.showPermissionRequest = false
        }
      })
    },

    triggerBasicNotification: function() {
      const options = {
        body: "This is a notification's body text. If it runs long it will automatically be truncated."
      }

      const notification = new Notification("Notification Title", options)
    },

    triggerNotificationWithIcon: function() {
      const options = {
        body: "So long, and thanks for all the fish!",
        icon: 'https://upload.wikimedia.org/wikipedia/commons/f/f1/Ruby_logo.png'
      }

      const notification = new Notification("New Message - Douglas Adams", options)
    },

    triggerDelayedNotification: function() {
      setTimeout(() => {
        this.triggerNotificationWithIcon()
      }, 1000)
    },

    triggerFocusLinkNotification: function() {
      const options = {
        body: "So long, and thanks for all the fish!",
        icon: 'https://upload.wikimedia.org/wikipedia/commons/f/f1/Ruby_logo.png'
      }

      setTimeout(() => {
        const notification = new Notification("New Message - Douglas Adams", options)

        notification.onclick = () => {
          window.focus()
        }
      }, 1000)
    },

    triggerBlurOnlyNotification: function() {
      const options = {
        body: "So long, and thanks for all the fish!",
        icon: 'https://upload.wikimedia.org/wikipedia/commons/f/f1/Ruby_logo.png'
      }

      setTimeout(() => {
        if (document.hidden) {
          const notification = new Notification("New Message - Douglas Adams", options)

          notification.onclick = () => {
            window.focus()
          }
        }
      }, 1000)
    },

    triggerClearOnClick: function() {
      const options = {
        body: "So long, and thanks for all the fish!",
        icon: 'https://upload.wikimedia.org/wikipedia/commons/f/f1/Ruby_logo.png'
      }

      setTimeout(() => {
        if (document.hidden) {
          const notification = new Notification("New Message - Douglas Adams", options)

          notification.onclick = () => {
            window.focus()
            notification.close()
          }
        }
      }, 1000)
    },

  }

}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Lato');

body {
  background-color: #FAFAFA;
}

#app {
  font-family: 'Lato', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 2em;
}

.permission-request {
  padding: 1.25em;
  margin: auto;
  max-width: 35em;

  background-color: rgba(241, 196, 15, .5);
  border: 2px solid rgba(241, 196, 15, 1);
  border-radius: 5px;
}

.underline-link {
  text-decoration: underline;
  cursor: pointer;
}

.notification-buttons {
  margin: 2em auto;
  max-width: 30em;
  /*Reset default ul padding*/
  padding: 0;
}

.button {
  display: inline-block;
  vertical-align: top;
  padding: 1em;
  margin: 1em;
  width: 5em;

  background-color: rgba(26, 188, 156, .3);
  border: 2px solid rgba(26, 188, 156, 1);
  border-radius: 5px;

  cursor: pointer;
}
</style>
