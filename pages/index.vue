<template>
  <section class="container-fluid">
    <div>
     <div class="row">
       <div class="col text-center bg1">
         <h1>Build your trading skills with IG Academy</h1>
         <p class="lead">Discover how to trade – or develop your knowledge – with free online courses, webinars and seminars. All from a world-leading company with over 40 years’ experience in the financial markets.        </p>
       <p class="cta"> <a class="button  button-green js_target" href="#" target="_self"> Join IG Academy </a> </p>
       </div>
     </div>

     <div class="row">
       <div class="col text-center bg2">
         <h2>Online trading courses</h2>
         <p class="lead"><button class="btn btn-primary" @click="subscribeUser()" v-show="!showSubscriptionBtn">Get alerts on new courses</button>

         <p class="lead"><button class="btn btn-warning" @click="unsubscribeUser()" v-show="showSubscriptionBtn">Stop alerts</button>

         <div>
           <div class="row">
             <div class="col" v-for="(item, index) in courses"  v-bind:key="item._id">
              
                <b-card
                :title="item.title"
                :img-src="item.image.url"
                img-alt="Image"
                img-top
                tag="article"
                style="max-width: 20rem;"
                class="mb-2"
              >
                <b-card-text>
{{item.intro}}
                </b-card-text>
   <p class="cta"> <nuxt-link class="button  button-green btn-sm" :to="'/courses/'+item._id"> Access</nuxt-link> </p>
                
              </b-card>
             </div>


           </div>
         
        </div>
       </div>
     </div>

    </div>
    <textarea class="form-control text11" v-if="subsJson!=''" v-model="subsJson"></textarea>
  </section>
</template>

<script>
import Logo from '~/components/Logo.vue'
import axios from 'axios'



const applicationServerPublicKey = 'BH5AyPXHTQYnRzuVZ7DHnMJqXxOm2FVo6y2HLDGTpGiTzL_XXcOKyGrqbO2TI0CpiHKCUXp9fHY2Eoks8AetmJs';

let isSubscribed = false;
let swRegistration = null;

export default {
  components: {
    Logo
  },
  data: function () {
    return {
      courses: [],
      showSubscriptionBtn: false,
      subsJson: ''
    }
  },
  beforeMount(){
    
 },
  methods: {
    urlB64ToUint8Array: function (base64String) {
      const padding = '='.repeat((4 - base64String.length % 4) % 4);
      const base64 = (base64String + padding)
        .replace(/\-/g, '+')
        .replace(/_/g, '/');

      const rawData = window.atob(base64);
      const outputArray = new Uint8Array(rawData.length);

      for (let i = 0; i < rawData.length; ++i) {
        outputArray[i] = rawData.charCodeAt(i);
      }
      return outputArray;
    },
    unsubscribeUser: function () {
      var that = this;
       swRegistration.pushManager.getSubscription()
        .then(function(subscription) {
          if (subscription) {
            return subscription.unsubscribe();
          }
        })
        .catch(function(error) {
          console.log('Error unsubscribing');
        })
        .then(function() {
          that.updateSubscriptionOnServer(null);

          console.log('User is unsubscribed.');

        });
    },
    updateSubscriptionOnServer: function(subscription) {
      if (subscription) {
        this.subsJson = JSON.stringify(subscription);
        this.showSubscriptionBtn = true;
      } else {
        this.showSubscriptionBtn = false;
      }

    },
    subscribeUser: function() {
      const applicationServerKey = this.urlB64ToUint8Array(applicationServerPublicKey);
      var that = this;
      swRegistration.pushManager.subscribe({
        userVisibleOnly: true,
        applicationServerKey: applicationServerKey
      })
      .then(function(subscription) {
        console.log('User is subscribed.');

        that.updateSubscriptionOnServer(subscription);

        isSubscribed = true;

        //updateBtn();
      })
      .catch(function(err) {
        console.log('Failed to subscribe the user: ');
        //updateBtn();
      });
    }
  },
  created: function () {
    // `this` points to the vm instance
    //let test = this.urlB64ToUint8Array('asdasd');
    
    if (process.browser) {
      let test = this.urlB64ToUint8Array('asdasd');


      if ('serviceWorker' in navigator && 'PushManager' in window) {
        console.log('Service Worker and Push is supported');

        navigator.serviceWorker.register('sw.js')
        .then(function(swReg) {
          console.log('Service Worker is registered');

          swRegistration = swReg;
          //initializeUI();
        })
        .catch(function(error) {
          console.error('Service Worker Error', error);
        });
      } else {
        console.warn('Push messaging is not supported');
        //pushButton.textContent = 'Push Not Supported';
      }


    }


  },
  computed: {
    window: function () { return 12; }
  },
  asyncData ({ params, error }) {
    return axios.get(`https://api.londongdhandball.co.uk/Courses`)
    .then((res) => {
      return { courses: res.data }
    })
    .catch((e) => {
      error({ statusCode: 404, message: 'Post not found' })
    })
  }
}
</script>

<style>
.card-title{
  font-weight: 200;
  font-size: 1.2em;
}
.card-text{
  font-size: 0.8em;
  color: #9a9a9a
}
.text11{
  font-size: 12px;
  margin-bottom: 100px;
  height:110px !important;
}
.lead{
  font-weight: 200;
}
.bg1{
  background: #f5f6f8;
  padding: 40px 80px;
}
h2{
  margin-bottom: 50px;
}
.bg2{
  background: white;
  padding: 40px 80px;
}

</style>
