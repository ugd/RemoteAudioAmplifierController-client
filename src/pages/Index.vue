<template>
  <q-page class="flex flex-center">
    <!-- <img
      alt="Quasar logo"
      src="~assets/quasar-logo-vertical.svg"
      style="width: 200px; height: 200px"
    > -->
    <q-card>
      <h1>Connected: {{ status }}</h1>
      <q-btn
        color="green"
        text-color="black"
        v-on:click="setLedStatus"
        label="Led Light"
      />
      <q-btn color="red" text-color="black" v-on:click="setPowerStatus" label="Power" />
      <!-- <q-btn label="Led" v-on:click="setLedStatus" ></q-btn> -->
      <q-slider
        v-model="mainVol"
        @change="setMainVol"
        label-value="Main Volume"
        markers
        :step="1"
        label-always
        :min="0"
        :max="63"
      />
      <q-slider
        v-model="bassVol"
        @change="setBassVol"
        label-value="Bass Volume"
        markers
        :step="1"
        label-always
        :min="0"
        :max="63"
      />
      <q-slider
        v-model="highVol"
        @change="setHighVol"
        label-value="High Volume"
        markers
        :step="1"
        label-always
        :min="0"
        :max="63"
      />
    </q-card>
  </q-page>
</template>

<script>
import { io } from "socket.io-client";
export default {
  name: "PageIndex",
  data() {
    return {
      socket: {},
      ledStatus: null,
      status: null,
      mainVol: 63,
      highVol: 63,
      bassVol: 63,
      power: false,
    };
  },
  methods: {
    setPowerStatus() {
      this.power = !this.power;
      if (this.power) {
        this.socket.emit("powerEventOn");
      } else {
        this.socket.emit("powerEventOff");
      }
    },
    setLedStatus() {
      this.ledStatus = !this.ledStatus;
      if (this.ledStatus) {
        this.socket.emit("setLedStatus", "on");
        console.log("Led Status", "Deschis");
      } else {
        this.socket.emit("setLedStatus", "off");
        console.log("Led Status+1 ", "Inchis");
      }
    },
    setMainVol() {
      let displayData = "B " + this.bassVol + " H " + this.highVol + " V " + this.mainVol;
      this.socket.emit("setMainVolume", this.mainVol);
      this.socket.emit("setdisplay", "               #");
      this.socket.emit("setdisplay", displayData);
    },
    setBassVol() {
      let displayData = "B " + this.bassVol + " H " + this.highVol + " V " + this.mainVol;
      this.socket.emit("setBassVolume", this.bassVol);
      this.socket.emit("setdisplay", "               #");
      this.socket.emit("setdisplay", displayData);
    },
    setHighVol() {
      let displayData = "B " + this.bassVol + " H " + this.highVol + " V " + this.mainVol;
      this.socket.emit("setHighVolume", this.highVol);
      this.socket.emit("setdisplay", "               #");
      this.socket.emit("setdisplay", displayData);
    },
  },
  created() {
    this.socket = io("http://10.10.10.123:9090/");
    this.socket.on("connect", () => {
      this.status = true;
      console.log("STATUS", "connected");
    });
    this.socket.on("disconnect", () => {
      this.status = false;
      console.log("STATUS", "disconnected");
    });
  },
};
</script>
