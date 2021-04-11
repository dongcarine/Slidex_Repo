<template>
  <v-container>
     <div class="vld-parent">
            <loading :active.sync="loading" 
            :can-cancel="false" 
            :on-cancel="onCancel"
            :color="'#1d1c45'"
            :loader="'dots'" 
            :opacity="0.8"
            :background-color="'#f2f2f5'"
            :is-full-page="fullPage"></loading>
            </div>
    <v-row class="text-center">
      <v-col cols="12">
        <v-img
          :src="require('../assets/icon.png')"
          class="my-3"
          contain
          height="100"
        />
      </v-col>

      <v-col class="mb-4">
        <h1 class="font-weight-bold mb-3" style="color:#3e0e69;font-size:1.3rem">
          Welcome to SlideX
        </h1>

        <p class="subheading font-weight-regular">
          Pay your parking fee below by entering your credit card <br>information below
          
          <!--<a
            href="https://community.vuetifyjs.com"
            target="_blank"
          >Discord Community</a>-->
        </p>
      </v-col>

      <v-col
        class="mb-5"
        cols="12"
      >
        <h2 class="headline font-weight-bold mb-3">
         Invoice
        </h2>
        <h3 v-if=" request==null" class="headline font-weight-bold mb-3" style="color:#C62828">
         Invalid request!
        </h3>
        <v-row justify="center" style="display:inline">
         
  <v-card
    :loading="loading"
    v-if=" request!=null"
    max-width="100%"
  >
    <template slot="progress">
      <v-progress-linear
        color="deep-purple"
        height="10"
        indeterminate
      ></v-progress-linear>
    </template>

    <v-img
      height="250"
      src="https://slidexstorage.blob.core.windows.net/slidexapppublicassets/posvaletlogo.jpg"
    ></v-img>

    <v-card-title>{{request.ServiceProvided}} by {{request.ServiceProvider}}</v-card-title>

    <v-card-text> 
      <v-row
        align="center"
        class="mx-0"
      >
        <v-rating
          :value="4.5"
          color="amber"
          dense
          half-increments
          readonly
          size="14"
        ></v-rating>

        <div class="grey--text ml-4">
          4.5 (413)
        </div>
       
      </v-row>
    <v-row
        align="center"
        class="mx-0"
      >
       <v-btn
      v-if="request.VehicleAuditArtifact!=null"
      color="blue-grey"
      class="ma-2 white--text"
      @click="showaudit=true"
    >
      View Audit
      <v-icon
        right
        dark
      >
        mdi-camera
      </v-icon>
    </v-btn>
    <v-btn
      v-if="request.Paid"
      color="green"
      class="ma-2 white--text"
      :href="request.ReceiptUrl" target="_blank"
      
    >
      Receipt
      <v-icon
        right
        dark
      >
        mdi-note
      </v-icon>
    </v-btn>
    </v-row>
     <v-row
        align="center"
        class="mx-0"
        v-if="request.TotalAmount!=0"
      >
      <div v-if="request.Paid" style="margin-top:20%;width:100%;height:50px;font-weight:500;font-size:5rem;color:green">PAID</div>
      <div style="margin-top:20%;width:100%;height:100px;font-weight:500;font-size:5rem;color:green">${{request.TotalAmount}}</div>
     </v-row>
      <v-row
        align="center"
        class="mx-0"
        v-if="request.TotalAmount==0"
      >
      <div v-if="request.Paid" style="margin-top:20%;width:100%;height:50px;font-weight:500;font-size:4rem;color:green">Complementary</div>
      <div style="margin-top:20%;width:100%;height:100px;font-weight:500;font-size:5rem;color:green">${{request.TotalAmount}}</div>
     </v-row>
    </v-card-text>

    <v-divider class="mx-4"></v-divider>
  <div v-if="!request.Paid">
    <v-card-title>Payment Info</v-card-title>
    <v-text-field v-model="firstname"
            placeholder="First Name"
          ></v-text-field>
    <v-text-field v-model="lastname"
      placeholder="Last Name"
    ></v-text-field>      

    <div id="card-element"></div>
    <br/>
    <v-divider></v-divider>
    <v-card-actions>
      <div class="text-center" id="payment-request-button">
        <v-btn 
          rounded
          @click="createPaymentMethod"
          color="primary"
          dark
        >
          PAY
        </v-btn>
      </div>
    </v-card-actions>
  </div>
  </v-card>
<v-row justify="center">
    <v-dialog
      
      v-model="showaudit"
      persistent
      max-width="500"
      overlay-opacity="0.8"
      v-if="showaudit"
    >
<v-card
    elevation="24"
    max-width="444"
    class="mx-auto"
    
  >
    
    <v-carousel
      :continuous="false"
      :cycle="cycle"
      :show-arrows="true"
      hide-delimiter-background
      delimiter-icon="mdi-car"
      height="300" v-if="this.request.VehicleAuditArtifact!=null"
    >
      <v-carousel-item
        v-for="(slide, i) in slides"
        :key="i"
        :src="slide.Path"
        reverse-transition="fade-transition"
        transition="fade-transition"
      >
       
      </v-carousel-item>
      <v-progress-linear v-if="loadingAck"
        :active="true"
        :indeterminate="true" stripped height="10" :background-opacity="request.Status!='Completed' && request.Status!='Cancelled' ? '0.5':'1'"
        absolute
        bottom
        color="#E65100"
      ></v-progress-linear>
      
    </v-carousel>
    
      
    <v-list two-line>
      <v-list-item>
  
        <v-list-item-action>
          <v-switch
            @change="confirmArtAck()"
            :v-model="artifactsack"
            :value="artifactsack"
            :disabled="auditdisabled"
            label="Acknowledge"
            inset
          >
         
          </v-switch>
          
        </v-list-item-action>
      </v-list-item>
    </v-list>
     
  </v-card>
  <v-btn
        color="orange darken-4"
        right
        
        
        @click="showaudit=false"
      >
        Close
      </v-btn>
    </v-dialog>
</v-row>
        </v-row>
      </v-col>


      <v-col
        class="mb-5"
        cols="12"
      >
        

        <v-row justify="center">
          <a
            v-for="(eco, i) in ecosystem"
            :key="i"
            :href="eco.href"
            class="subheading mx-3"
            target="_blank"
          >
            {{eco.text}}
          </a>
        </v-row>
      </v-col>
    </v-row>
    <ConfirmDialog 
    :showcancel="true" 
    v-on:cancelconfirmdialog="hideConfirmDialog()"
    v-on:agreeconfirmdialog="acknowledgeArtifiacts()"
    :title="'Acknowledge Audit?'"
    :showdialog="confirmacknowledgment"
    :message="'Please browse through all the artifiacts. After acknowledging, you will not be able to reverse this action.'"
    :cancellabel="'Cancel'"
    :agreelabel="'Agree'"
  
  />
  <ConfirmDialog
    :showcancel="false" 
    v-on:cancelconfirmdialog="genericdialog=false"
    v-on:agreeconfirmdialog="genericdialog=false"
    :title="generictitle"
    :showdialog="genericdialog"
    :message="genericmessage"
    :cancellabel="'Cancel'"
    :agreelabel="'OK'"
  
  />
  </v-container>
</template>

<script>
import Loading from 'vue-loading-overlay';
import ConfirmDialog from './ConfirmDialog'
 import axios from 'axios';
    // Import stylesheet
    import 'vue-loading-overlay/dist/vue-loading.css';
  export default {
    name: 'Payment',
   
    data: () => ({
      ecosystem: [
        {
          text: 'Visit SlideX',
          href: 'https://www.slidex.io',
        },
        {
          text: 'Download APP',
          href: 'https://github.com/vuetifyjs/vuetify',
        },
        {
          text: 'Disclaimer',
          href: 'https://github.com/vuetifyjs/awesome-vuetify',
        },
      ],
       stripeAPIToken: 'pk_test_0GvhYWJu8DUailPKvuhGBqs700THzQ6NqI',
       stripe: '',
        elements: '',
        card: '',
        showaudit:false,
        fullPage:true,
        confirmacknowledgment:false,
        slides:[],
        genericdialog:null,
        genericmessage:null,
        generictitle:null,
        loadingAck:false,
        artifactsack:false,
        loading:false,
        
        auditdisabled:false,
        request:[],
        firstname:null,
        lastname:null
    }),
    props:['servicerequestid'],
    components:{
      Loading,ConfirmDialog
    },
    methods:{
       includeStripe( URL, callback ){
        let documentTag = document, tag = 'script',
            object = documentTag.createElement(tag),
            scriptTag = documentTag.getElementsByTagName(tag)[0];
        object.src = '//' + URL;
        if (callback) { object.addEventListener('load', function (e) { callback(null, e); }, false); }
        scriptTag.parentNode.insertBefore(object, scriptTag);
      },
      onCancel(){

      },
      acknowledgeArtifiacts(){
              this.confirmacknowledgment=false;
              this.loadingAck=true;
              let obj={
                ServiceRequestId:this.request.ServiceRequestId
              }
              this.requestSender('POST', '/api/ServiceRequest/AcknowledgeVehicleAuditNonMember', obj)
                    .then(() => {
                         this.loadingAck=false;
                         this.auditdisabled=true;
                         this.showaudit=false;
                         this.genericdialog=true
                         this.genericmessage="Vehicle audit acknowledged successfully!"
                         this.generictitle="Success"
                      
                        this.$refs.itemsSliding.close()
                    })
                    .catch(error => {
                            this.showaudit=false;
                            this.loadingAck=false;
                            console.log("login error:"+JSON.stringify(error.response.data))
                            this.genericdialog=true
                            this.genericmessage="Operation failed!"
                            this.generictitle="Failure"
                    })
               
            },
      configureStripe(){
          this.stripe = window.Stripe( this.stripeAPIToken );

          this.elements = this.stripe.elements();
          
          this.card = this.elements.create('card',{
            style: {
            base: {
              iconColor: '#c4f0ff',
              color: '#3e0e69',
              fontWeight: '600',
              width:'50%',
              fontFamily: 'Roboto, Open Sans, Segoe UI, sans-serif',
              fontSize: '18px',
              fontSmoothing: 'antialiased',
              ':-webkit-autofill': {
                color: '#fce883',
              },
              '::placeholder': {
                color: '#b58adb',
              },
            },
            invalid: {
              iconColor: '#f02259',
              color: '#f02259',
            },
          },

          });

          this.card.mount('#card-element');
      },
       createPaymentMethod(){
         this.loading=true
          this.stripe.createPaymentMethod({
            type: 'card',
            card: this.card,
            billing_details: {
              // Include any additional collected billing details.
              name: this.firstname +" "+this.lastname,
            },
          })
          .then(this.stripePaymentMethodHandler);
       },
       auditAcknowledged(arts){
              var rep=true
              arts.forEach(element => {
                if(element.Acknowledged==false) rep= false
              });
              return rep
        },
        confirmArtAck(){
        this.confirmacknowledgment=true;
        this.artifactsack=true;
      },
       requestSender(method, url, params) {
                const data = Object.entries(params)
                    .map(([key, val]) => `${key}=${encodeURIComponent(val)}`)
                    .join('&');

                const requestURL = 'https://living-api.slidex.io' + url;

                let headers;
                if (requestURL === 'https://living-api.slidex.io/Token' || requestURL === 'https://living-api.slidex.io/api/Account/Register') {
                    headers = {'Content-Type': 'application/x-www-form-urlencoded'}
                } else {
                    headers = {
                        'Authorization': 'Bearer ' + localStorage.getItem('token'),
                        'Content-Type': 'application/x-www-form-urlencoded'
                    }
                }
                return axios({
                    method: method,
                    url: requestURL,
                    headers: headers,
                    data: data,
                })
            },
      hideConfirmDialog(){
        this.confirmacknowledgment=false;
        this.artifactsack=false;
      },
       getServiceRequestInfo(id){
          this.loading=true
                    return new Promise((resolve, reject) => {
                    axios({
                        method: 'GET',
                        url: 'https://living-api.slidex.io/api/ServiceRequest/GetServiceRequestById?ServiceRequestId='+id,
                        headers: {
                            'Authorization': 'Bearer ' + localStorage.getItem('token'),
                            'Content-Type': 'application/x-www-form-urlencoded'
                        }
                        })
                        .then((resp) => {
                            this.loading=false
                            console.log("AUTH QRCODE:"+JSON.stringify(resp.data))
                            this.request=resp.data
                            resolve(resp)
                            
                        })
                        .catch((err) => {
                          this.request=null
                            this.loading=false
                            
                            console.log("Error dashboard data" + JSON.stringify(err));
                             //this.cannotdecode=true
                            reject(err);
                        });
                })
           
       },
       async stripePaymentMethodHandler(result) {
        if (result.error) {
          // Show error in payment form
          this.loading=false
          console.log("ERROR:"+JSON.stringify(result.error))
           this.genericdialog=true
           this.genericmessage=result.error
           this.generictitle="Failure"
        } else {
          // Otherwise send paymentMethod.id to your server (see Step 4)
          const res = await fetch('https://living-api.slidex.io/api/Payment/DirectPayment', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({
              payment_method_id: result.paymentMethod.id,
              service_request_id:this.servicerequestid
            }),
          })
          const paymentResponse = await res.json();

          // Handle server response (see Step 4)
          this.handleServerResponse(paymentResponse);
        }
      },
      async handleServerResponse(response) {
        console.log("DIRECTPAYMENT RESPONSE:"+JSON.stringify(response))
        this.loading=false
        if (response.error) {
          // Show error from server on payment form
          console.log("ERROR:"+JSON.stringify(response.error))
          this.genericdialog=true
           this.genericmessage=response.error
           this.generictitle="Failure"
        } else if (response.requires_action) {
          // Use Stripe.js to handle the required card action
          console.log("REQUIRES ACTION:"+JSON.stringify(response.requires_action))
          const { error: errorAction, paymentIntent } =
            await window.stripe.handleCardAction(response.payment_intent_client_secret);

          if (errorAction) {
            // Show error from Stripe.js in payment form
            console.log("ERROR ACTION:"+JSON.stringify(errorAction))
            this.genericdialog=true
            this.genericmessage=errorAction
            this.generictitle="Failure"
          } else {
            // The card action has been handled
            // The PaymentIntent can be confirmed again on the server
            const serverResponse = await fetch('https://living-api.slidex.io/api/Payment/DirectPayment', {
              method: 'POST',
              headers: { 'Content-Type': 'application/json' },
              body: JSON.stringify({ payment_intent_id: paymentIntent.id,service_request_id:this.servicerequestid })
            });
            this.handleServerResponse(await serverResponse.json());
          }
        } else {
          // Show success message
          console.log("SUCCESS")
          this.genericdialog=true
          this.genericmessage="Thanks for your payment. Your receipt has been textec to your phone"
          this.generictitle="Success"
          this.getServiceRequestInfo(this.servicerequestid)
          .then(()=>{
            console.log("AUDIT ARTIFACTES:"+JSON.stringify(this.request.VehicleAuditArtifact))
          this.slides=this.request.VehicleAuditArtifact
          this.artifactsack=this.auditAcknowledged(this.request.VehicleAuditArtifact)
          
          this.auditdisabled=this.auditAcknowledged(this.request.VehicleAuditArtifact)
          })
        }
      }
    },
    mounted(){
      this.includeStripe('js.stripe.com/v3/', function(){
          this.configureStripe();
      }.bind(this) );
      
    },
    created(){
      console.log("LENGTH:"+this.request.length)
      this.getServiceRequestInfo(this.servicerequestid)
      .then(()=>{
        console.log("AUDIT ARTIFACTES:"+JSON.stringify(this.request.VehicleAuditArtifact))
      this.slides=this.request.VehicleAuditArtifact
      this.artifactsack=this.auditAcknowledged(this.request.VehicleAuditArtifact)
      
      this.auditdisabled=this.auditAcknowledged(this.request.VehicleAuditArtifact)
      })
    }
      
      
      
  
  }
</script>
