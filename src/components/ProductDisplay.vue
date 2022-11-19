<template>
  <div v-if="loading === true">
     <div class="background">
                 <div class="container"><div class="loader"></div></div>
      </div>
  </div>
  <div v-else>
    <div v-if="datas">



      <div v-if="gender === 'male'">

              <div class="background latarMale">

                  <div class="container">
                    <div class="imgContainer">
                      <img :src="datas.image" :alt="datas.title" />
                    </div>


                                <div class="textContainer">


                                        <div class="textContainerRating">
                                            <div class="textTitle colorMale"><h3>{{ datas.title }}</h3></div>

                                            <div class="ratingText">
                                              <div class="textSubTitle">
                                                <p>{{ datas.category }}</p>
                                              </div>

                                                <div class="ratingNumber">
                                                  <div class="numberStar">{{ datas.rating.rate }}/5</div>
                                                  <div class="checkboxStar">
                                                    <div class="checkboxStarDua" v-for="countRating in 5" :key="countRating">
                                                      <div class="ratingRadio radioMale borderMale" v-if="countRating <= starRating"></div>
                                                      <div class="nonRating borderMale" v-else></div>
                                                    </div>
                                                  </div>
                                                </div>

                                              </div>

                                              <div class="description"><p>{{ datas.description }}</p></div>
                                          </div>



                                            <div class="textContainerButton">
                                              <div class="textPrice colorMale">
                                              <h3>$ {{datas.price}}</h3>
                                              </div>
                                              <div class="button">
                                                <button class="btnColor radioMale borderMale">Buy Now</button>
                                                <button class="btnBorder colorMale borderMale" @click.prevent="countProduct">Next product</button>
                                              </div>
                                            </div>
                                        </div>
                                </div>

                    </div>

                </div>

                <div v-else>
                    <div class="background latarFemale">

                          <div class="container">
                            <div class="imgContainer">
                              <img :src="datas.image" :alt="datas.title" />
                            </div>


                                <div class="textContainer">


                                        <div class="textContainerRating">
                                            <div class="textTitle colorFemale"><h3>{{ datas.title }}</h3></div>

                                            <div class="ratingText">
                                              <div class="textSubTitle">
                                                <p>{{ datas.category }}</p>
                                              </div>

                                                <div class="ratingNumber">
                                                  <div class="numberStar">{{ datas.rating.rate }}/5</div>
                                                  <div class="checkboxStar">
                                                    <div class="checkboxStarDua" v-for="countRating in 5" :key="countRating">
                                                      <div class="ratingRadio radioFemale borderFemale" v-if="countRating <= starRating"></div>
                                                      <div class="nonRating borderFemale" v-else></div>
                                                    </div>
                                                  </div>
                                                </div>

                                              </div>

                                              <div class="description">{{ truncate(longText, 500, '...')  }}</div>
                                          </div>



                                            <div class="textContainerButton">
                                              <div class="textPrice colorFemale">
                                              <h3>$ {{datas.price}}</h3>
                                              </div>
                                              <div class="button">
                                                <button class="btnColor  radioFemale borderFemale">Buy Now</button>
                                                <button class="btnBorder colorFemale borderFemale" @click.prevent="countProduct">Next product</button>
                                              </div>
                                            </div>
                                        </div>
                                </div>

                    </div>


                </div>
      </div>
   
      
          <div v-else>
                 <div class="background latarNotFound">

                          <div class="container">
                            
                                            <div class="textContainerNotFound">
                                                  <div class="textNotFound colorNotFound">
                                                  <h3>This product is unavailable to show</h3>
                                                  </div>
                                                  <div class="buttonNotFound">
                                                    <button class="btnNotFound colorNotFound borderNotFound" @click.prevent="countProduct">Next product</button>
                                                  </div>
                                            </div>
                            
                            </div>
                  </div>
            </div>

    </div>
   

</template>

<script>
export default {
  name: "ProductDisplay",
  data() {
    return {
      datas: [],
      loading: true,
      qtyProduct: 1,
      gender: "",
      countRating: 1,
      starRating: "",
      longText:""

    };
  },
  mounted() {
    this.getData();
  },
  methods: {
    countProduct() {
      if (this.qtyProduct === 20) {
        this.qtyProduct = 1;
        console.log(this.qtyProduct);
        this.getData();
      } else {
        this.qtyProduct = this.qtyProduct + 1;
        console.log(this.qtyProduct);
        this.getData();
      }
    },
    getData() {
      this.loading = true;
      setTimeout(() => {
        fetch("https://fakestoreapi.com/products/" + this.qtyProduct)
          .then(response => response.json())
          .then(data => {
            this.loading = false;
            this.datas = data;
            if (this.datas.category === `men's clothing` || this.datas.category == `women's clothing`) {
              if (this.datas.category === `men's clothing`) {
                this.gender = "male";
                console.log(this.gender);
                this.datas = data;
                this.starRating = Math.round(this.datas.rating.rate);
              } else if (this.datas.category == `women's clothing`) {
                this.gender = "female";
                console.log(this.gender);
                this.datas = data;
                this.starRating = Math.round(this.datas.rating.rate);
                this.longText = this.datas.description;

                
              }
            } else {
              this.datas = false;
            }
          })
          .catch(err => console.log(err.message));
      }, 1000);
    },
      truncate: function (text, length, suffix) {
            if (text.length > length) {
                return text.substring(0, length) + suffix;
            } else {
                return text;
            }
        },
  },
};
</script>
<style scoped>
@import "../assets/style/page.css";
</style>
