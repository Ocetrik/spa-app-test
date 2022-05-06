<template>
  <div class="user-table">
    <div class="user-table__search-input">
      <input v-model="searchQuery" placeholder="Поиск" type="text" />
      <img src="@/assets/icons/Search.png" alt="" />
    </div>
    <div class="user-table__header">
      <div class="header-id">ID</div>
      <img
        @click="sortIdOfUsers"
        src="@/assets/icons/Arrow.svg"
        class="header-arrow"
        alt=""
      />
      <div class="header-title">Заголовок</div>
      <img
        @click="sortTitleOfUsers"
        src="@/assets/icons/Arrow.svg"
        class="header-arrow"
        alt=""
      />
      <div class="header-description">Описание</div>
      <img
        @click="sortBodyOfUsers"
        src="@/assets/icons/Arrow.svg"
        class="header-arrow"
        alt=""
      />
    </div>
    <div
      v-for="user in sortedAndSearchedUsers"
      :key="user.id"
      class="user-table__content"
    >
      <div class="content-id">{{ user.id }}</div>
      <div class="content-title">{{ user.title }}</div>
      <div class="content-body">{{ user.body }}</div>
    </div>
    <div class="user-table__pagination">
      <button
        @click="prevPage"
        :disabled="isDisablePrevBtn"
        class="pagination__prev-btn btn"
      >
        Назад
      </button>
      <div
        v-for="pageNumber in totalPages"
        :key="pageNumber"
        :class="{ 'current-page': page === pageNumber }"
        @click="changePage(pageNumber)"
        class="pagination__page"
      >
        {{ pageNumber }}
      </div>
      <button
        @click="nextPage"
        :disabled="isDisableNextBtn"
        class="pagination__next-btn btn"
      >
        Далее
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "UserTable",
  data() {
    return {
      users: [],
      page: 1,
      totalPages: 0,
      limit: 2,
      searchQuery: "",
      isNeedSorted: false,
    };
  },
  methods: {

  setLocation(curLoc) {
    try {
      history.pushState(null, curLoc, curLoc)
      return
    } catch(e) {
      console.log(e)
    }
    location.hash = '#' + curLoc
  },

    async fetchUsers() {
      try {
        const response = await axios.get(
          "https://jsonplaceholder.typicode.com/posts?_limit=10",
          {
            params: {
              _page: this.page,
              _limit: this.limit,
            },
          }
        );
        this.users = response.data;
        this.totalPages = Math.ceil(this.users.length / this.limit);
        this.setLocation("/"+ this.page)
      } catch (e) {
        alert("Error");
      }
    },
    changePage(pageNumber) {
      this.page = pageNumber;
      this.fetchUsers();
    },
    nextPage() {
      this.page += 1;
      this.fetchUsers();
    },
    prevPage() {
      this.page -= 1;
      this.fetchUsers();
    },
    sortTitleOfUsers() {
      this.users.sort((user1, user2) => user1.title.localeCompare(user2.title));
    },
    sortBodyOfUsers() {
      this.users.sort((user1, user2) => user1.body.localeCompare(user2.body));
    },
    sortIdOfUsers() {
      this.users.sort((user1, user2) => user1.id - user2.id);
    },
  },
  created() {
    this.fetchUsers();
  },
  computed: {
    sortedAndSearchedUsers() {
      return this.users.filter((user) => user.title.includes(this.searchQuery));
    },
    isDisableNextBtn() {
      return this.page === 5
    },
    isDisablePrevBtn() {
      return this.page === 1
    }
  },
};
</script>

<style lang="scss" scoped>
.header-arrow {
  margin-left: 40px;
  cursor: pointer;
}
.user-table {
  max-width: 1100px;
  margin: 0 auto;
  margin-top: 30px;
}
.user-table__search-input {
  position: relative;
  img {
    position: absolute;
    top: 15px;
    left: 590px;
    z-index: 5;
  }
}
input {
  width: 100%;
  min-height: 52px;
  max-width: 631px;
  background-color: #5a5c66;
  position: relative;
  color: #b3b7bf;
  padding: 15px;
  padding-left: 25px;
  margin-bottom: 15px;
}
.user-table__header {
  display: flex;
  background: #474955;
  box-shadow: 0px 4px 27px rgba(230, 231, 234, 0.78);
  font-family: "Roboto";
  font-style: normal;
  font-weight: 600;
  font-size: 14px;
  line-height: 19px;
  color: #ffffff;
  padding: 19px 0 16px 23px;
}
.header-title {
  margin-left: 200px;
}
.header-description {
  margin-left: 450px;
}
.user-table__content {
  display: flex;
}
.content-id {
  border: 1px solid #e3e6ec;
  min-width: 110px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.content-title {
  border: 1px solid #e3e6ec;
  min-width: 529px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.content-body {
  border: 1px solid #e3e6ec;
  min-width: 432px;
  padding: 20px;
}
.user-table__pagination {
  display: flex;
  justify-content: center;
  gap: 15px;
  padding-top: 24px;
  font-family: "Roboto";
  font-style: italic;
  font-weight: 700;
  font-size: 18px;
  line-height: 25px;
  flex-direction: row;
}
.pagination__next-btn {
  margin-left: 380px;
}
.btn {
  font-family: "Roboto";
  font-style: normal;
  font-weight: 500;
  font-size: 24px;
  line-height: 33px;
  color: #474955;
  background: #ffffff;
  border: #ffffff;
  cursor: pointer;
  margin-top: -7px;
}
.pagination__prev-btn {
  margin-right: 380px;
}
.pagination__page {
  cursor: pointer;
}
.current-page {
  color: #7ebc3c;
}
</style>