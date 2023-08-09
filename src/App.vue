<template>
    <div class="app">
        <header>
            <h1>Anime<strong> Encyclopedia</strong></h1>

            <form class="search-box" @submit.prevent="HandleSearch">
              <input type="search" 
                  class="search-field" 
                  placeholder="Search...."
                  required 
                  v-model="search_query" />
                
            </form>
        </header>
        <main>
            <div class="cards" v-if="animelist.length > 0">
              <Card v-for="anime in animelist" :key="anime.mal_id" :anime="anime"/>
            </div>
            <div class="no-results" v-else>
              <h3>Sorry, we have no results.....</h3>
            </div>
        </main>
      </div>
</template>


<script>
import { ref } from 'vue';
import Card from './components/Card.vue';

export default {
  name: 'app',
    components: {
      Card
    },
  // data () {
  //   return {
      
  //   }
  // },

  setup () {
    const search_query = ref("");
    const animelist = ref([]);

    
    const HandleSearch = async () => {
      const response = await fetch(`https://api.jikan.moe/v4/anime?q=${search_query.value}`)

      if (response.ok) {
        const { data }  = await response.json()
        animelist.value = data
        console.info(animelist)
      }
      search_query.value = "";
    }
    return {
      Card,
      search_query,
      animelist,
      HandleSearch
    }
  }
}
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;


  font-family: 'fira Sans', sans-serif;
}


a {
  text-decoration: none;
}

header {
  padding-top: 50px;
  padding-bottom: 50px;
  background-color: hsl(274, 17%, 62%);
  opacity: 100%;
  border-radius: 5px;


  h1 {
    color:#fbfbfb;
    font-size:42px;
    font-weight: 400;
    text-align: center;
    text-transform: uppercase;
    margin-bottom: 30px;
    

    strong {
      color: #5f0d95;
    }

    &:hover {
      color:#5f0d95;
      
    }

  }

  .search-box {
      display: flex;
      justify-content: center;
      padding-left: 30px;
      padding-right: 30px;
    
    .search-field {
      appearance: none;
      background: none;
      border: none;
      outline: none;

      background-color: #F3F3F3;
      box-shadow: 8px 4px 8px rgba(0, 0, 0, 0.15);

      display: block;
      width: 100%;
      max-width: 600px;
      padding: 15px;
      border-radius: 8px;


      color: #313131;
      font-size: 20px;

      transition: 0.4s;

      &::placeholder {
        color: #AAA;
      }

      &:focus, &:valid {
        color: #FFF;
        background-color: #313131;
        box-shadow: 0px 0px 0px rgba(0, 0, 0, 0.15);
      }
    }
  }
} 
main {
  max-width: 1200px;
  margin: 0 auto;
  padding-left:30px;
  padding-right: 30px;


  .cards {
    display: flex;
    flex-wrap: wrap;
    margin: 0 -8px;
  }
}

.no-results{
    font-size:30px;
    font-weight: 400;
    text-align: center;
    margin-bottom: 30px;

}

</style>
