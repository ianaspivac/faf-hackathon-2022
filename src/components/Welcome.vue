<template>
  <div class="item">
    <div class="column">
      <h1>PLAC3 H0LD3R</h1>
    </div>
    <div >
      <div>
        <div class="user underline">
          <div class="avatar"></div>
          <div>Us3rnam3</div>
        </div>
      </div>
    </div>
    <div>
      <div>
        <div class="info row balance underline">
          <div>
            <div>Balance:</div>
          </div>
          <div class="flex-bottom">
            <div class="statistics-data">
              {{ balance }}
            </div>
          </div>
        </div>
      </div>
      <div>
        <div class="statistics row underline">
          <div class="flex-bottom">
            <div class="statistics-data">
              {{ victims }}
            </div>
          </div>
          <div>
            <div>caught victims</div>
          </div>
        </div>
        <div class="victims statistics row underline">
          <div>Time played as a victim</div>
          <div class="statistics-data">
            <div class="flex-bottom">
              {{ time }}
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="btn">
      <RouterLink to="/game">ENTER THE GAME</RouterLink>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      balance: "100",
      victims: "4",
      time: "4:20",
      tag: "//01. <Home/>",
      tag1: "//02. <game/>"
    };
  },
  computed: {
    points() {
      return 100;
    },
  },
};
</script>
<style scoped>
.tags {
  color: #4d4e53;
  font-size: 10px;
}
.user {
  font-size: 28px;
}
.balance,
.statistics {
  font-size: 18px;
}

.statistics-data {
  border: 1px solid #00ff93;
  border-bottom: none;
  padding-left: 3px;
  padding-right: 3px;
}
.row {
  display: flex;
  width: 50%;
  margin-top: 30px;
  margin-bottom: 30px;
}
.btn {
  height: 200px;
  width: 200px;
  border-radius: 50%;
  background-color: #00ff93;
  font-size: 26px;
  padding: 40px 60px;
  cursor: pointer;
  margin: 10px auto;
}
.btn a {
  color: black;
}

.underline {
  border-bottom: 1px solid #00ff93;
}

.column {
  display: flex;
  flex-direction: column;
}

.first-tags {
  justify-content: space-between;
}
.row.balance,
.row.victims {
  margin-left: auto;
}
.victims .padding {
  padding: 0 20px;
}
.row.balance,
.statistics.row {
  width: 200px;
  display: flex;
  justify-content: space-between;
}
.row.victims {
  width: 250px;
  display: flex;
  justify-content: space-between;
}

</style>
