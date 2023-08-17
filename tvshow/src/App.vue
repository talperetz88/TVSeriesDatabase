<template>
  <div id="App">
    <main>
      <div class="home">
        <div class="home-left">
          <div class="tv-top">
            <h1>The TV Series Database</h1>
            <div class="search-tv">
              <input type="text" class="search-input" placeholder="Search..." v-model="tvName" @keypress="searchCity"/>
            </div>
          </div>
          <div class="tv-list">
          <h1>Search Results</h1>
          <div v-show="tvShows.length === 0" class="no-results"><span>No Results</span></div>
          <div v-show="tvShows.length > 0" class="selected-tv-list" :key="tvShow.show.id" v-for="tvShow in tvShows">
              <div class="card">              
                <img :src="tvShow.show.image.medium" alt="Poster">
                <div class="tv-info">
                  <h1>{{tvShow.show.name}}</h1>
                  <div class="details">
                    <p>{{tvShow.show.rating.average}}</p>
                    <p>{{tvShow.show.genres}}</p>
                  </div>
                </div>
                <svg class="svgCl" @click="addToFavorite(tvShow.show.externals.imdb)" v-if=checkFev(tvShow.show.externals.imdb) width="20" height="18" viewBox="0 0 20 18" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path id="Vector (Stroke)" fill-rule="evenodd" clip-rule="evenodd" d="M2.60958 2.60858C1.86847 3.3501 1.5 4.32553 1.5 5.3075C1.5 6.28805 1.86754 7.25437 2.61033 7.99717L10 15.3868L17.3897 7.99717C18.1319 7.25497 18.5 6.27892 18.5 5.3075C18.5 4.31711 18.1328 3.35429 17.3756 2.61369L17.3711 2.60934C15.8839 1.13047 13.487 1.13023 11.9995 2.60864L10.5203 4.08783C10.3794 4.22879 10.1881 4.30783 9.98873 4.30749C9.78937 4.30716 9.59836 4.22747 9.45787 4.08603L7.99116 2.60932L7.98958 2.60775C6.50225 1.13047 4.09656 1.13075 2.60958 2.60858ZM1.55117 1.54568C3.62372 -0.515226 6.97628 -0.515226 9.04883 1.54568L9.05213 1.54896L9.9918 2.49504L10.9397 1.54717L10.9412 1.54568C13.013 -0.514493 16.3539 -0.515226 18.4266 1.54348C19.4879 2.5826 20 3.93884 20 5.3075C20 6.65607 19.4881 8.02003 18.4503 9.05783L10.598 16.9101C10.56 16.9481 10.5079 17.0003 10.416 17.0615C10.1641 17.2295 9.8359 17.2295 9.58397 17.0615C9.49212 17.0003 9.43997 16.9481 9.40195 16.9101L1.54967 9.05783C0.512465 8.02062 0 6.66694 0 5.3075C0 3.9498 0.511284 2.58556 1.54967 1.54717L1.55117 1.54568Z" fill="#161721"/>
                </svg>
                
                <svg class="svgCl" @click="removeFromFavorite(tvShow.show.externals.imdb)" v-if=!checkFev(tvShow.show.externals.imdb) xmlns="http://www.w3.org/2000/svg" width="20" height="18" viewBox="0 0 20 18" fill="none">
                <path fill-rule="evenodd" clip-rule="evenodd" d="M1.55117 1.54568C3.62372 -0.515226 6.97628 -0.515226 9.04883 1.54568L9.05213 1.54896L9.9918 2.49504L10.9397 1.54717L10.9412 1.54568C13.013 -0.514493 16.3539 -0.515226 18.4266 1.54348C19.4879 2.5826 20 3.93884 20 5.3075C20 6.65607 19.4881 8.02003 18.4503 9.05783L10.598 16.9101C10.56 16.9481 10.5079 17.0003 10.416 17.0615C10.1641 17.2295 9.8359 17.2295 9.58397 17.0615C9.49212 17.0003 9.43997 16.9481 9.40195 16.9101L1.54967 9.05783C0.512465 8.02062 0 6.66694 0 5.3075C0 3.9498 0.511284 2.58556 1.54967 1.54717L1.55117 1.54568Z" fill="#F04D4D"/>
                </svg>
              </div>
            </div>
          </div>
        </div>
        <div class="home-right">
          <h1></h1>
          <div class="my-tv-list">
          <h1>My Favorites</h1>
          <div class="my-selected-tv-list" :key="myTv.externals.imdb" v-for="myTv in myTvList" >
            <div class="card">              
              <img :src="myTv.image.medium" alt="Poster">
              <div class="tv-info">
                <h1>{{myTv.name}}</h1>
                <div class="details">
                  <p>{{myTv.rating.average}}</p>
                  <p>{{myTv.genres}}</p>
                </div>
              </div>
              <svg class="svgCl" @click="removeFromFavorite(myTv.externals.imdb)" xmlns="http://www.w3.org/2000/svg" width="20" height="18" viewBox="0 0 20 18" fill="none">
              <path fill-rule="evenodd" clip-rule="evenodd" d="M1.55117 1.54568C3.62372 -0.515226 6.97628 -0.515226 9.04883 1.54568L9.05213 1.54896L9.9918 2.49504L10.9397 1.54717L10.9412 1.54568C13.013 -0.514493 16.3539 -0.515226 18.4266 1.54348C19.4879 2.5826 20 3.93884 20 5.3075C20 6.65607 19.4881 8.02003 18.4503 9.05783L10.598 16.9101C10.56 16.9481 10.5079 17.0003 10.416 17.0615C10.1641 17.2295 9.8359 17.2295 9.58397 17.0615C9.49212 17.0003 9.43997 16.9481 9.40195 16.9101L1.54967 9.05783C0.512465 8.02062 0 6.66694 0 5.3075C0 3.9498 0.511284 2.58556 1.54967 1.54717L1.55117 1.54568Z" fill="#F04D4D"/>
              </svg>
            </div>
          </div>
        </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {
  },
  data(){
    return{
      tvShows:[],
      tvName:'',
      myTvList:[],
      svgColor: '#F04D4D;'
    }
  },
  methods: {
    searchCity(e){      
      if(e.key === 'Enter'){
        this.getTvShowData(this.tvName);
      }
    },
    async getTvShowData(tvName){
      const res = await fetch(`https://api.tvmaze.com/search/shows?q=${tvName}`)
      const finalRes = await res.json();
      console.log(finalRes);
      this.tvShows = finalRes.filter(tv => {
        if(tv.show.image != null) {
          tv.show.genres = tv.show.genres.join(" | ")
          return tv ;
        }
      })
    },
    async addToFavorite(imdbID){
      if (this.isItemInArray(imdbID) !== -1)
        return;
      const res = await fetch(`https://api.tvmaze.com/lookup/shows?imdb=${imdbID}`);
      
      const finalRes = await res.json();
      finalRes.genres = finalRes.genres.join(" | ")
      this.myTvList.push(finalRes)
      window.sessionStorage.setItem('myTvList', JSON.stringify(this.myTvList));
    },
    removeFromFavorite(imdbID){
      const index = this.isItemInArray(imdbID)
      if (index === -1)
        return
      this.myTvList.splice(index, 1);
      window.sessionStorage.setItem('myTvList', JSON.stringify(this.myTvList));
      
    },
    getMyTvList(){
      this.myTvList = window.sessionStorage.getItem('myTvList') ? JSON.parse(window.sessionStorage.getItem('myTvList')) : []
    },
    isItemInArray(imdbID) {
      return this.myTvList.findIndex(myTv => {
        return myTv.externals.imdb === imdbID;
      });
    },
    checkFev(imdbID){
      return this.isItemInArray(imdbID) === -1 ? true : false
    }
    
  },
  mounted(){
    // window.sessionStorage.removeItem('myTvList');
    this.getMyTvList();
  }
}
</script>

<style>
.home{
  display: flex;
  flex-direction: row;
  justify-content: center;
  column-gap: 50px;
  flex-wrap: wrap;
}

.home-left{
  display: flex;
  flex-direction: column;
  /* justify-content: center; */
  gap: 8px;
}
.home-right{
  display: flex;
  flex-direction: column;
  /* justify-content: center;
  gap: 8px; */
  row-gap: 77px
}

.no-results{
  color: #2C3E50;
font-family: Inter;
font-size: 16px;
font-style: normal;
font-weight: 400;
line-height: normal;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
* {
    margin:0;
    padding: 0;
  }

  main{
    min-height: 100vh;
    padding: 25px;
  }
  .tv-top{
    display: inline-flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 8px;
  }
  .tv-top h1{
    color: #2C3E50;
    font-family: Inter;
    font-size: 30px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
  }

  .search-tv{
    width: 300px;
    height: 25px;
    border-radius: 6px;
    border: 1px solid #B4B4B4;
  }
  .search-tv .search-input{
    width: 300px;
    height: 25px;
    color:  #B4B4B4;
    font-family: Inter;
    font-size: 16px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    appearance: none;
    border: none;
    outline: none;
    border-radius: 6px;
  }
  .search-tv .serach-input:focus{
    box-shadow: 0 0 16px rgb(0,0,0,0.25);
    background-color: azure;
    border-radius: 16px 0 16px 0;
  }
  .tv-wrap{
    display: flex;
    flex-direction: row;
    justify-content: center;
    flex-wrap: wrap;
    gap:50px;
  }
  .tv-list{
    /* margin: 15px; */
    display: inline-flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 8px;
    min-width: 400px;
  }
  .tv-list h1,.my-tv-list h1{
    color: #2C3E50;
    font-family: Inter;
    font-size: 22px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
  }
  .my-tv-list{
    /* margin: 15px; */
    display: inline-flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 8px;
    min-width: 400px;

  }
  .svgCl{
    padding: 3px;
  }
  .selected-tv-list{
    /* padding: 15px; */
    display: inline-flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 8px;

  }
  .card {
    border-radius: 8px;
    background: #FFF;
    box-shadow: 0px 0px 8px 0px rgba(0, 0, 0, 0.12);  
    display: flex;
    flex-direction: row;
    width: 400px;
    height: 120px;
    align-items: flex-start;
    gap: 8px;
    padding: 0 8px;
  }
  .card img{
    width: 75px;
    height: 105px;
    flex-shrink: 0;
    border-radius: 4px;
    background: url(<path-to-image>), lightgray 50% / cover no-repeat;
    align-self:center
  }
  .card .tv-info{
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: flex-start;
    flex: 1 0 0;
    align-self: stretch;
    text-align: left;
    padding: 3px;
  }
  .card .tv-info h1{
    color: #2C3E50;
    font-family: Inter;
    font-size: 22px;
    font-style: normal;
    font-weight: 500;
    line-height: normal;
    justify-content: start;
  }
  .card .tv-info p{
    color: #2C3E50;
    font-family: Inter;
    font-size: 14px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
  }
  .card .tv-info .details{
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: flex-start;
    flex: 1 0 0;
    align-self: stretch;
    text-align: left;
  }
  .card .tv-info p:last-child{
    justify-content: end;
  }
  @media (max-width: 768px) {
  .home {
    flex-direction: column; /* Switch to a column layout for mobile */
    align-items: center; /* Center items horizontally */
    gap: 20px; /* Increase spacing between elements */
  }

  /* Reset the width and max-width for cards to 100% */
  .card {
    width: 100%;
    max-width: 100%;
  }
  .my-selected-tv-list{
    width: 80%;
  }
  /* Adjust font sizes for smaller screens */
  .tv-top h1 {
    font-size: 24px;
  }

  .tv-list h1,
  .my-tv-list h1 {
    font-size: 18px;
  }

  .card .tv-info h1 {
    font-size: 18px;
  }

  .card .tv-info p {
    font-size: 12px;
  }
}
  
</style>
