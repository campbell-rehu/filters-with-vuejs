<template>
  <div id="app">
    <div class="selector">
      <label for="filterSelection">Filter by: </label>
      <select id="filterSelection" v-model="filterSelection">
        <option value="all">All</option>
        <option value="electronics">Electronics</option>
        <option value="accessories">Accessories</option>
      </select>
    </div>
    <div class="selector">
      <label for="orderBySelection">Order by: </label>
      <select id="orderBySelection" v-model="orderBySelection">
        <option value="name">Name</option>
        <option value="lowToHigh">Lowest to Highest Price</option>
        <option value="highToLow">Highest to Lowest Price</option>
      </select>
    </div>
    <h1 id="selection">
      <b>{{filterSelection + " Products" | capitalize}}</b>
    </h1>
    <ul>
      <li v-for="product in filtered">
        {{product.name | capitalize}} - {{product.price | currency}}
      </li>
    </ul>
  </div>
</template>

<script>

export default {
  name: 'app',
  computed: {
    filtered() {
      /* 
        Here I am storing filterSelection, orderBySelection and products locally.
        I found that calling orderBySelection without assigning it to a local variable
        returned an object that turned out to be the entire <select> element.
        I'm not sure why or how it did that.
      */
      var filterSelection = this.filterSelection;
      var orderBySelection = this.orderBySelection;
      var filteredProducts = this.products;

      /* 
          Here we only filter the products if the user has selected 
          something other than 'all' in the filter <select> element.       
      */
      if (filterSelection != 'all') {
        filteredProducts = filteredProducts.filter((product) => {
          return product.category === filterSelection;
        });
      }

      /*
        If the user selects ordering from Lowest to Highest Price,
        sort accordingly.
      */
      if (orderBySelection === 'lowToHigh') {
          return filteredProducts.sort((productA, productB) => {
            /*
              Extracting the price from the product object
            */
            var priceA = parseFloat(productA.price);
            var priceB = parseFloat(productB.price);
            if (priceA > priceB) {
              return 1;
            } else if (priceA < priceB) {
              return -1;
            }
            return 0;
          });
      /*
        Same here for ordering from Highest to Lowest Price.
      */
      } else if (orderBySelection === 'highToLow') {
        return filteredProducts.sort((productA, productB) => {
          var priceA = parseFloat(productA.price);
          var priceB = parseFloat(productB.price);
          if (priceB > priceA) {
            return 1;
          } else if (priceB < priceA) {
            return -1;
          }
          return 0;
        });
      /*
        The default sort option is by name alphabetically.
      */
      } else {
        return filteredProducts.sort((productA, productB) => {
          var nameA = productA.name.toLowerCase();
          var nameB = productB.name.toLowerCase();

          if (nameA > nameB) {
            return 1;
          } else if (nameA < nameB) {
            return -1;
          }
          return 0;
        });
      }
      
      return filteredProducts;
    }
  },
  filters: {
    capitalize(value) {
      if (!value) return '';
      
      value = value.toString();
      
      return value.charAt(0).toUpperCase() + value.slice(1);
    },
    currency(value) {
      if (!value) {
        return '';
      } else if (isNaN(value)) { // Check that the value is a number
        return 'POA';
      }
      value = value.toString();

      // Find the decimal place in the value
      var decimalPlace = value.indexOf('.');

      // If there is no decimal place, add a decimal place and two zeroes
      if (decimalPlace === -1) {
        value += '.00';
      } else {
        // Find the number of digits after the decimal place
        var postDecimalDigits = value.slice(decimalPlace, value.length-1).length;
        
        // If there are more than two digits after the decimal place, round it to two.
        if ( postDecimalDigits > 2 ) {
          
          /* Find the number of digits before the decimal place.
            This is to use in the toPrecision() method which needs
            to know the total number of digits in the number. 
          */
          var preDecimalDigits = value.slice(0, decimalPlace).length;
          var floatValue = parseFloat(value).toPrecision(preDecimalDigits+2);
          value = floatValue;
        }
      }

      // Add the dollar sign
      return '$' + value;
    }
  },
  data() {
    return {
      filterSelection: 'all',
      orderBySelection: 'name',
      products: [
        {
          name: 'microphone', price: '25', category: 'electronics'
        },
        {
          name: 'laptop case', price: '15', category: 'accessories'
        },
        {
          name: 'screen cleaner', price: '17', category: 'accessories'
        },
        {
          name: 'laptop charger', price: '70', category: 'electronics'
        },
        {
          name: 'mouse', price: '40', category: 'electronics'
        },
        {
          name: 'earphones', price: '20', category: 'electronics'
        },
        {
          name: 'monitor', price: '120', category: 'electronics'
        }
      ]
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.selector {
  margin: 20px;
}

ul {
  list-style: none;
  padding: 0;
}
</style>
