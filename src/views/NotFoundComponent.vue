<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar color="secondary">
        <ion-title>Time Fighter</ion-title>
        <ion-buttons slot="primary">
          <ion-button color="primary" fill="solid" @click="info">
            <ion-icon :icon="infoIcon"></ion-icon>
          </ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      TODO
    </ion-content>
  </ion-page>
</template>


<script>
import {
  alertController,
  IonButton,
  IonIcon,
  IonButtons,
  IonContent,
  IonHeader,
  IonPage,
  IonTitle,
  IonToolbar,
  IonCol,
  IonRow,
  IonGrid,
  toastController
} from '@ionic/vue';
import {defineComponent} from 'vue';
import {informationCircleOutline} from "ionicons/icons";
import { createAnimation } from "@ionic/vue";

const INITIAL_TIME = 60

export default defineComponent({
  name: 'HomePage',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonButtons,
    IonButton,
    IonIcon,
    IonCol,
    IonRow,
    IonGrid
  },
  setup() {
    return {
      infoIcon: informationCircleOutline,
      started: false,
      counterInterval: null
    }
  },
  data() {
    return {
      score: 0,
      timeLeft: INITIAL_TIME
    }
  },
  watch: {
    timeLeft: function(newTimeLeft) {
      if (newTimeLeft <= 0) {
        this.started = false
        this.timeLeft = INITIAL_TIME
        clearInterval(this.counterInterval)
        this.showResult()
        this.score = 0
      }
    }
  },
  methods: {
    bounce() {
      const animation = createAnimation()
      animation.addElement(document.getElementById('tapMeButton'))
          .duration(2000)
          .fromTo('transform','scale(2.0)','scale(1.0)')
      animation.play();
    },
    blink() {
      const animation = createAnimation()
      animation.addElement(document.getElementById('counterText'))
          .duration(500)
          .fromTo('opacity','0','1')
      animation.play();
    },
    async info() {
      const alert = await alertController
          .create({
            header: 'Time Fighter 1.0',
            subHeader: 'Creat per Jordi Riba',
            message: 'Podeu trobar el codi font a: <a href="https://github.com/Riba00/ComptadorIonic">https://github.com/Riba00/ComptadorIonic',
            buttons: ['OK'],
          });
      await alert.present();
    },
    tap() {
      this.bounce()
      this.blink()
      this.score++
      if (!this.started) {
        this.counterInterval = setInterval(() => {
          this.timeLeft--
        }, 1000)
        this.started = true
      }
    },
    async showResult() {
      const toast = await toastController.create({
        color: 'dark',
        duration: 4000,
        message: `TEMPS ACABAT!!! El resultat és -> ${this.score}` ,
        showCloseButton: true
      });
      await toast.present();
    },
  },
});
</script>

<style scoped>
#container {
  text-align: center;

  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;

  color: #8c8c8c;

  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>
