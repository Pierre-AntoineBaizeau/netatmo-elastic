
<template>
  
  <div id="app" class="product">
    <div class="searchbar">
        <span class="search">Search</span>
        <input type="text" 
            class="search-input" 
            @keyup.prevent="search"
            v-model="query" />
    </div>
<!-- You ask me to retrieve the data for a product using the SKU “24-MB02” but the are no sku “24-MB02” so you can cherche any one you want-->
    <p><b>Put into the search bar :</b> 24-WB04</p>

    <div  v-if="data" class="card-row">
        <div v-for="(value, index) in data" 
        :key="index"
        :ref="`card_${index}`"
            class="card">
        
        <!-- the image in the json file show nothing so i replace the image by one find in google but if you want 
        to see the json file image you have just to change the comment line -->
        <!-- <img class="card-image" :src="value._source.image" alt="json file image"> -->
        <img class="card-image" src="https://www.bocage.fr/media/catalog/product/cache/1/image/9df78eab33525d08d6e5fb8d27136e95/2/0/20699610482_1_1_1.jpg" alt="a bag">
        <div class="card-right"> 

            
            <h3 class="card-title">{{value._source.name}}</h3>

            <pre>   {{value._source.description | 
                replace("&lt;p&gt;",'') | 
                replace("&lt;/p&gt;",'') | 
                replace("&lt;ul&gt;",'') | 
                replace("&lt;/ul&gt;",'') | 
                replace("&lt;li&gt;",'- ') | 
                replace("&lt;/li&gt;",'')
            }}
            </pre>
            <p class="price">price : {{value._source.price}}</p>
            <div class="select-and-add">
                <div>
                    <select v-model="selected">
                    <option v-for="option in options" v-bind:key="option.text">
                        {{ option.text }}
                    </option>
                    </select>
                    
                        <span class="selected">Selected: {{ selected }}</span>
                        <p>quantity : {{value._source.stock.qty}}</p>
                </div>
                <button class="add" v-on:click="value._source.stock.qty -= selected">Add to Cart </button>
                        
            </div>
        </div>


        </div>
    </div>
  </div>
  

</template>


<script>
export default {
  data() {
    return {
      query: '',
      data: [],
          selected: '0',
      options: [
      { text: '0'},
      { text: '1'},
      { text: '2'},
      { text: '3'}
    ],
    }
  },
  methods: {
    search() {
        this.axios.get('http://localhost:5000/search?q='+this.query)
              .then(response => {
                this.data = response.data;
          })
    },
  },
  filters: {
    replace: function (st, rep, repWith) {
      const result = st.split(rep).join(repWith)
      return result;
    }
  },
  
}

</script>

<style>


.product {
    text-align: center;
   
}

.searchbar {
    width: 400px;
    display: flex;
    margin: 20px 64%;
}

.search {
    background-color: rgb(90, 90, 189);
    color: rgb(241, 241, 241);
    border-radius: 10px 0 0 10px;
    padding: 10px 25px;
    cursor: pointer;
}

.search-input {
    width: 300px;
    border-radius: 0 10px 10px 0;
    border-color: black;
}



.card {
    justify-content: center;
    align-items: center;  
    flex-wrap: wrap;
    height: 570px;
    width: 80%;
    margin: 0px auto;
    border: none !important;
}
.card-image {
    max-width: 40%;
    box-shadow: 0px 2px 4px 0px rgba(0,0,0,0.5);
}

.card-right {
    width: 40%;
}

pre {
    white-space:pre-wrap;
    text-align: left;
}
.card-text {
    font-size: 14px;
    color: rgba(0, 0, 0, 0.7);
}
.card-title {
      font-family: Serif;
}

.price {
    text-align: left;
}

.select-and-add {
    display: flex;
    flex-wrap: wrap;
}

.selected {
    margin-left: 10px;
}

.add {
    background-color: rgb(90, 90, 189);
    color: rgb(241, 241, 241);
    border-radius: 10px;
    padding: 0px 25px;
    cursor: pointer;
    box-shadow: 0px 4px 8px 1px rgba(0,0,0,0.5);
    border: none;
    margin-left: 20%;
    margin-top: -10px;
    height: 50px;
}

@media (max-width: 870px) {
  .card {
      flex-wrap: initial;
      height: auto;
  }
}
</style>
