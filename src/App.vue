<template>
  <div class="container">
    <div>
      <input
        class="input" 
        v-model.trim="searchValue" 
        type="text"
        placeholder="шукай тут..."
      >
    </div>
    <div class="list">
      <list :posts="filteredPosts" v-if="!isLoading" />
      <div v-else>Завантаження...</div>
    </div>
    <div class="page-selector">
      <div
        v-for="pageNum in totalPages"
        :key="pageNum"
        class="page-selector__item"
        :class="{'active': page === pageNum}"
        @click="chanchePage(pageNum)"
      >
        {{ pageNum }}
      </div>
    </div>
    <div v-if="filteredPosts.length < 1">
      Нічого не знайдено...
    </div>
  </div>
  <footer class="footer">
    <div class="footer__container">
      <div class="footer__self-box">
        <img src="./img/av.jpg" alt="avatar" class="footer__img">
        <span>mykhailo novikov</span>
      </div>  
      <span class="footer__link-box">
        <span>githab link:</span>
        <a class="footer__link" href="https://github.com/novikovm18">github.com/novikovm18</a>
      </span>
    </div>
  </footer>
</template>

<script>
import List from "@/components/List";
import axios from "axios";

export default {
  components: {
    List
  },
  data() {
    return {
      posts: [],
      searchValue: '',
      isLoading: false,
      page: 1,
      limit: 10,
      totalPages: 10,
    }
  },
  methods: {
    chanchePage(pageNum) {
      this.page = pageNum;
    },
    async fetchPosts() {
      const BASE_URL = `https://data.jsdelivr.com/v1/stats/packages`;
      try {
        this.isLoading = true;
        const responce = await axios.get(`${BASE_URL}`, {
          params: {
            limit: this.limit,
            page: this.page
          }
        });    
        // this.totalPages = Math.ceil(responce.headers['x-total-count'] / this.limit);
        this.posts = await responce.data;
      } catch (e) {
          alert('Помилка завантаження...')
      } finally {
        this.isLoading = false;
      }
    }
  },
    mounted() {
      this.fetchPosts()
    },
  computed: {
    filteredPosts() {
      return this.posts.filter((post) => post.name.toLowerCase().includes(this.searchValue.toLowerCase()));
    }
  },
  watch: {
    page() {
      this.fetchPosts();
    }
  }
}
</script>

<style lang="scss">
body {
  margin: 0;
  padding: 0;
  font-family:'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
  background-image: url(./img/bg.jpg);
  color: rgb(275, 250, 5);
}
.container {
  position: relative;
  margin: 0 auto;
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  max-width: 1200px;
  height: calc(100vh-3rem);
  font-size: 1.8rem;
  line-height: 2.2rem;

  @media (max-width:767px) {
    padding: 20px;
    max-width: 1200px;
    font-size: 1.6rem;
  }
}

.input {
  padding-left: 10px;
  font-size: 1.8rem;
  line-height: 2rem;
  margin-bottom: 2rem;
  border-radius: 8px;
  color: rgb(275, 250, 5);
  background: rgba(125, 125, 125, 0.5);

  @media (max-width:767px) {
    font-size: 1.4rem;
    line-height: 1.8rem;
    margin-bottom: 1rem;
  }
}

.list {
  margin-bottom: 2rem;

  @media (max-width:767px) {
    max-width: 300px;
  }
}

.posts {
  display: flex;
  flex-direction: column;
}

.post {
  cursor: pointer;
  padding: 0.5rem;
  transition: 300ms;
  border-radius: 4px;

  &:hover {
    transform: scale(1.2, 1.2);
  }

  &:nth-child(2n+1) {
    background: rgba(125, 125, 125, 0.5);
  }
}

.page-selector {
  bottom: 1rem;
  display: flex;
  gap: 0.2rem;
  margin-bottom: 2.8rem;

  &__item {
    padding: 0.1rem;
    border: 1px solid black;
    border-radius: 4px;
    cursor: pointer;
    transition: 250ms;
    width: 3rem;
    height: 4rem;
    display: flex;
    align-items: center;
    justify-content: center;

    @media (max-width:767px) {
      width: 1.8rem;
      height: 3rem;
      font-size: 1rem;
    }

    &:hover {
      border: 1px solid rgb(275, 250, 5);
    }
  }
}


.active {
  background: rgba(125, 125, 125, 0.5);
  border: 1px solid rgb(275, 250, 5);
}

.footer {
  position: fixed;
  bottom: 0;
  width: 100%;
  height: 3rem;
  background: rgba(125, 125, 125, 0.25);
  color: lightcyan;
  transition: 400ms;

  &:hover {
    background: rgba(75, 75, 75, 0.85);
  }

  &__container {
    margin: 0 auto;
    padding: 2px 20px;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
    max-width: 1200px;
    font-size: 1.2rem;
  }

  &__self-box {
    display: flex;
    gap: 1rem;
    align-items: center;
    justify-content: space-between;
  }

  &__img {
    width: 3rem;
    height: 2.8rem;
    border-radius: 50%;
  }

  &__link-box {
    display: flex;
    gap: 0.5rem;
    align-items: center;
    justify-content: space-between;
  }

  &__link {
    text-decoration: none;
    color:lightcyan;
  }
}


.modal{
  position: fixed;
  width: 50%;
  height: 30%;
  top: 30%;
  left: 25%;
  display: flex;
  flex-direction: column;
  background-image: url(./img/bg_modal.jpg);
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  border: 1px solid black;
  border-radius: 4px;

  @media (max-width:767px) {
    width: 80%;
    height: max-content;
    top: 30%;
    left: 10%;
  }

  &__container {
    top: 0;
    left: 0;
    position: fixed;
    background: rgba(125, 125, 125, 0.75);
    width: 100%;
    height: 100vh;
    
    &:focus {
      outline: none;
    }
  }
  
  &__close {
    position: absolute;
    width: max-content;
    right: 4px;
    font-size: 1.2rem;
    
    &:hover{
      font-weight: 900;
    }
  }

  &__content {
    margin-top: 12px;
    padding: 1rem;
    display: flex;
    flex-direction: column;
  }
}
</style>
