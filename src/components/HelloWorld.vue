<template>
  <div class="page justify-content-center">
    <div class="box">
      <div style="display: flex">
        <div class="credit-card" >
          <div class="card-side front" :style="[isFocused ? {'transform': 'rotateY(180deg)'} : {}]" >

           <!-- <div class="row">
              <div class="col-12">
                <input placeholder="asdasd">
              </div>
            </div>-->
s
            <input :value="getCardType" disabled style="background: transparent; border: 0"/>
            <div class="row">
              <div class="col-12">
               <transition name="fade">
                 <input style="width: 100%; padding: 10px 15px; border-radius: 5px; background: transparent; color:white; font-size: 25px;" :style="[cardNumberFocus ? {'border' : '1px solid white'} : {'border':'0'}]" :value="getCardNumberHash" disabled>

               </transition>
              </div>
            </div>


            <div class="row">
              <div class="col-7">
                <label class="input-card-label">Card Holder</label>
                <input class="card-input" style="background: transparent;color:white; border:0" :value="cardHolder" disabled  :style="[cardHolderFocus ? {'border' : '1px solid white'} : {'border':'0'}]">
              </div>
              <div class="col-5">
                <label class="input-card-label" style="background: transparent">Expires</label>
                <input class="card-input"  style="background: transparent; color:white; border:0" :value="monthAndYear" :style="[expiresFocus ? {'border' : '1px solid white'} : {'border':'0'}]" disabled>
              </div>
            </div>


          </div>
          <div class="card-side back" :style="[isFocused ? {'transform': 'rotateY(0deg)'} : {}]">
            <div class="row">

            </div>
            <div class="row">

            </div>
            <div class="row">
              <div class="col-7">
              </div>
              <div class="col-5">
                <label class="input-card-label" style="background: transparent" >Cvv</label>
                <input class="card-input"  style="background: transparent; color:white; border:0" :value="cVV" disabled :style="[isFocused ? {'border' : '1px solid white'} : {'border':'0'}]">
              </div>
            </div>

          </div>
        </div>
      </div>
      <div class="card-div-box justify-content-center">
        <div class="card-input-div">
          <label class="input-card-label">Card Number</label>
          <br>
          <input class="card-input" @input="change"
                 @change="change" maxlength="16"  @focusin="cardNumberFocus=!cardNumberFocus" @focusout="cardNumberFocus=!cardNumberFocus" />
        </div>
      </div>
      <div class="card-div-box justify-content-center">
        <div class="card-input-div">
          <label class="input-card-label">Card Name</label>
          <br>
          <input class="card-input" v-model="cardHolder" maxlength="30" @focusin="cardHolderFocus=!cardHolderFocus" @focusout="cardHolderFocus=!cardHolderFocus"/>
        </div>
      </div>
      <div class="card-div-box justify-content-center">

        <div class="card-input-div">
         <div class="row">
            <div class="col-9">
              <div class="row">
                <div class="col-6">
                  <label class="input-card-label">Expiration date</label>
                  <br>
                  <select class="select-menu" v-model="month" @focusin="expiresFocus=!expiresFocus" @focusout="expiresFocus=!expiresFocus">
                    <option >Month</option>
                    <option v-for="(month,index) in months" v-bind:key="index">
                      {{month}}
                    </option>
                  </select>
                </div>
                <div class="col-6">
                  <br/>
                  <select class="select-menu" v-model="year" @focusin="expiresFocus=!expiresFocus" @focusout="expiresFocus=!expiresFocus">
                    <option value="0">Year:</option>
                    <option v-for="(year,index) in years" v-bind:key="index" :value="yearValue(year)">{{ year }}</option>
                  </select>
                </div>
              </div>
            </div>
           <div class="col-3">
             <label class="input-card-label">CVV</label>
             <br>
             <input class="card-input" @focusin="isFocused=!isFocused" @focusout="isFocused=!isFocused" @input="changeCvv"
                    @change="changeCvv" maxlength="4"/>
           </div>
         </div>
        </div>

      </div>
      <div class="card-div-box justify-content-center">
        <div class="card-input-div">
          <button class="btn btn-primary"> submit </button>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
  import VueCreditCardValidation from 'vue-credit-card-validation';
export default {
  name: 'PracaDomowa',
  data(){
      return{
        isFocused:false,
        months:['01','02','03','04','05','06','07','08','09','10','11','12'],
        cardInput:null,
        cardHolder:'',

        month:null,
        year:null,
        cVV:null,
        cardHolderFocus:false,
        expiresFocus:false,
        cardNumberFocus:false,


      }
  },
  components:{
    // eslint-disable-next-line vue/no-unused-components
    VueCreditCardValidation
  },
  computed:{

    getCardNumberHash(){
      let cardNumberHash="";
      if(this.cardInput!==null)
      {
        console.log(this.cardInput.length)
        for(let i=0;i<this.cardInput.length;i++)
        {
          if(this.cardInput[i]!==" ")
          {
            cardNumberHash+=this.cardInput[i];
          }
        }
        while(cardNumberHash.length<16)
        {

          cardNumberHash+="#"
        }
        cardNumberHash = cardNumberHash.replace(/(.{4})/g, '$1 ')
      }

      return cardNumberHash
    },
    getCardType () {
      if(this.cardInput!==null)
      {
        if(this.cardInput==='')
          return ''
        let number = this.cardInput;

        let re = new RegExp("^4");
        if (number.match(re) != null) return "visa";

        re = new RegExp("^(34|37)");
        if (number.match(re) != null) return "amex";

        re = new RegExp("^5[1-5]");
        if (number.match(re) != null) return "mastercard";

        re = new RegExp("^6011");
        if (number.match(re) != null) return "discover";

        re = new RegExp('^9792')
        if (number.match(re) != null) return 'troy'

        return "visa";
      }else return ''
    },
    years () {
      const year = new Date().getFullYear()
      return Array.from({length: year - 1950}, (value, index) => 1951 + index)
    },
    monthAndYear(){
      if(this.month===null && this.year===null)
      {
        return 'MM/YY'
      }
      else if(this.month===null)
      {
        return 'MM/'+this.year
      }
      else if(this.year===null)
      {
        return this.month+'/'+'YY'
      } else
        return this.month+'/'+this.year
    }
  },

  methods:{
    yearValue(v){
      v = String(v).slice(-2);
      v = Number(v);
      return v
    },
    changeCvv(event){
      let val = event.target.value.trim()
      // It can only be positive integer or empty, and the rule can be modified according to the requirement.
      if (/^[1-9]\d*$|^$/.test(val)) {
        this.cVV = val
      } else {
        event.target.value = this.cVV
      }
    },
    change (event) {

        let val = event.target.value.trim()
        // It can only be positive integer or empty, and the rule can be modified according to the requirement.
        if (/^[1-9]\d*$|^$/.test(val)) {
          this.cardInput = val
        } else {
          event.target.value = this.cardInput
        }
        this.cardInput = this.cardInput.replace(/(.{4})/g, '$1 ')
      }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s;
  }
  .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
    opacity: 0;
  }
  .page{
    width: 100%;
    height: 100vh;
    background: url("https://media.istockphoto.com/vectors/black-abstract-background-vector-id686873178?k=20&m=686873178&s=612x612&w=0&h=yXXJxVm-9Q8MZfhoKf3hZ0FIhj4U_ZU9XUxTbWn_YIs=");
    background-repeat: no-repeat;
    background-size: cover;
    display: flex;
  }
.box{
  background-color: white;
  width: 48%;
  height: 60vh;
  margin:auto;
  border-radius: 2%;
  box-shadow: 0 0 100px 2px grey;
  display: grid;
  grid-template-rows: 10rem auto;
}
  .credit-card{

  }
.card-input-div{
  border: gray;
  width: 70%;
}
  .card-input{
    border: 1px solid rgba(199, 199, 199, 0.98);
   width: 100%;
    border-radius: 5px;
    padding: 10px 5px;

  }
  .flip{
    transform: rotateY(180deg);
  }

  .card-side {
    transition: all 0.8s ease;
    backface-visibility: hidden;
    position: absolute;
    top: 3%;
    left: 35%;
    padding:2rem;
    color: white;
    background: url("https://besthqwallpapers.com/Uploads/21-5-2020/134003/thumb-3d-black-texture-geometric-black-texture-black-abstraction-background-3d-abstraction-black-texture-creative-black-background.jpg");
    background-repeat: no-repeat;
    background-size: cover;
    border-radius: 10px;
    height: 30vh;
    width: 30%;
    box-shadow: 0 0 60px 4px black;
    perspective: 1000px;
  }

  .card-side.back {
    transform: rotateY(-180deg);
    background: url("https://besthqwallpapers.com/Uploads/21-5-2020/134003/thumb-3d-black-texture-geometric-black-texture-black-abstraction-background-3d-abstraction-black-texture-creative-black-background.jpg");
    background-repeat: no-repeat;
    background-size: cover;
    display: grid;
  }

  .card-side.front {
    background: url("https://besthqwallpapers.com/Uploads/21-5-2020/134003/thumb-3d-black-texture-geometric-black-texture-black-abstraction-background-3d-abstraction-black-texture-creative-black-background.jpg");
    background-repeat: no-repeat;
    background-size: cover;
    display: grid;
  }
  .card-div-box{
    display: flex;
  }
  .input-card-label{
    float: left;
    font-size: 13px;
    font-weight: bold;
    margin:0;

  }
  .btn{
    width: 100%;
  }
  .select-menu{
    padding: 10px 5px;
    border-radius: 5px;
    border: 1px solid rgba(199, 199, 199, 0.98);
    width: 100%;
  }
  .back-side{
    transform: rotateY(180deg);
  }
</style>
