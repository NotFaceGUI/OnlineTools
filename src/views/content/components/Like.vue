<template>
  <div id="main-content" style="display: inline-block; vertical-align: middle">
    <error v-show="show"></error>
    <div style="display: inline-block">
      <input
        type="checkbox"
        class="checkbox"
        :id="id"
        @click="choose()"
        v-model="state"
      />
      <label :for="id">
        <svg
          id="heart-svg"
          viewBox="467 392 58 57"
          xmlns="http://www.w3.org/2000/svg"
        >
          <g
            id="Group"
            fill="none"
            fill-rule="evenodd"
            transform="translate(467 392)"
          >
            <path
              d="M29.144 20.773c-.063-.13-4.227-8.67-11.44-2.59C7.63 28.795 28.94 43.256 29.143 43.394c.204-.138 21.513-14.6 11.44-25.213-7.214-6.08-11.377 2.46-11.44 2.59z"
              id="heart"
              fill="#FFFFFF"
            />
            <circle
              id="main-circ"
              fill="#E2264D"
              opacity="0"
              cx="29.5"
              cy="29.5"
              r="1.5"
            />

            <g id="grp7" opacity="0" transform="translate(7 6)">
              <circle id="oval1" fill="#9CD8C3" cx="2" cy="6" r="2" />
              <circle id="oval2" fill="#8CE8C3" cx="5" cy="2" r="2" />
            </g>

            <g id="grp6" opacity="0" transform="translate(0 28)">
              <circle id="oval1" fill="#CC8EF5" cx="2" cy="7" r="2" />
              <circle id="oval2" fill="#91D2FA" cx="3" cy="2" r="2" />
            </g>

            <g id="grp3" opacity="0" transform="translate(52 28)">
              <circle id="oval2" fill="#9CD8C3" cx="2" cy="7" r="2" />
              <circle id="oval1" fill="#8CE8C3" cx="4" cy="2" r="2" />
            </g>

            <g id="grp2" opacity="0" transform="translate(44 6)">
              <circle id="oval2" fill="#CC8EF5" cx="5" cy="6" r="2" />
              <circle id="oval1" fill="#CC8EF5" cx="2" cy="2" r="2" />
            </g>

            <g id="grp5" opacity="0" transform="translate(14 50)">
              <circle id="oval1" fill="#91D2FA" cx="6" cy="5" r="2" />
              <circle id="oval2" fill="#91D2FA" cx="2" cy="2" r="2" />
            </g>

            <g id="grp4" opacity="0" transform="translate(35 50)">
              <circle id="oval1" fill="#F48EA7" cx="6" cy="5" r="2" />
              <circle id="oval2" fill="#F48EA7" cx="2" cy="2" r="2" />
            </g>

            <g id="grp1" opacity="0" transform="translate(24)">
              <circle id="oval1" fill="#9FC7FA" cx="2.5" cy="3" r="2" />
              <circle id="oval2" fill="#9FC7FA" cx="7.5" cy="2" r="2" />
            </g>
          </g>
        </svg>
      </label>
    </div>
  </div>
</template>

<script>
import Error from "../../../components/Error.vue";
export default {
  components: { Error },
  name: "like",
  props: {
    id: Number,
    tab: Object,
    topState: Boolean,
  },
  data() {
    return {
      state: false,
      show: false,
      username: sessionStorage.getItem("username"),
      ask: "",
    };
  },
  mounted() {
    setTimeout(() => {
      this.state = this.topState;
    }, 500);
    // let a = setInterval(() => {
    //   this.state = this.topState;

    //   if (this.state == this.topState) {
    //     clearInterval(a);
    //   }
    // }, 100);
  },
  methods: {
    add() {
      this.tab.numberOfCollections += 1;
      this.ask = "update";
      // 发送更新请求
      this.req();
    },
    reduce() {
      this.tab.numberOfCollections -= 1;
      // 发送删除请求
      this.ask = "delete";
      this.req();
    },
    choose() {
      if (this.username == null) {
        this.show = true;
        setTimeout(() => {
          setTimeout(() => {
            this.show = false;
          }, 500);
          this.state = false;
        }, 500);
      } else {
        if (this.state) {
          this.reduce();
        } else {
          this.add();
        }
      }
    },
    req() {
      this.axios
        .get("/online/api/UpdateServlet", {
          params: {
            ask: this.ask,
            username: this.username,
            card_id: this.tab.card_id,
          },
        })
        .then((res) => {
          // console.log(res);
          console.log(res);
        });
    },
  },
};
</script>

<style lang="scss" scoped>
h1 {
  font-size: 14px;
  font-weight: 400;
  margin: 10px 0 0 0;
  color: #888888;
}

a {
  font-size: 12px;
  font-weight: bold;
  margin-top: 10px;
  display: inline-block;
  text-decoration: none;
  color: #008a68;
}

svg {
  cursor: pointer;
  overflow: visible;
  width: 40px;
  #heart {
    transform-origin: center;
    animation: animateHeartOut 0.3s linear forwards;
  }
  #main-circ {
    transform-origin: 29.5px 29.5px;
  }
}

.checkbox {
  display: none;
}

.checkbox:checked + label svg {
  #heart {
    transform: scale(0.2);
    fill: #e2264d;
    animation: animateHeart 0.3s linear forwards 0.25s;
  }
  #main-circ {
    transition: all 2s;
    animation: animateCircle 0.3s linear forwards;
    opacity: 1;
  }
  #grp1 {
    opacity: 1;
    transition: 0.1s all 0.3s;
    #oval1 {
      transform: scale(0) translate(0, -30px);
      transform-origin: 0 0 0;
      transition: 0.5s transform 0.3s;
    }
    #oval2 {
      transform: scale(0) translate(10px, -50px);
      transform-origin: 0 0 0;
      transition: 1.5s transform 0.3s;
    }
  }
  #grp2 {
    opacity: 1;
    transition: 0.1s all 0.3s;
    #oval1 {
      transform: scale(0) translate(30px, -15px);
      transform-origin: 0 0 0;
      transition: 0.5s transform 0.3s;
    }
    #oval2 {
      transform: scale(0) translate(60px, -15px);
      transform-origin: 0 0 0;
      transition: 1.5s transform 0.3s;
    }
  }
  #grp3 {
    opacity: 1;
    transition: 0.1s all 0.3s;
    #oval1 {
      transform: scale(0) translate(30px, 0px);
      transform-origin: 0 0 0;
      transition: 0.5s transform 0.3s;
    }
    #oval2 {
      transform: scale(0) translate(60px, 10px);
      transform-origin: 0 0 0;
      transition: 1.5s transform 0.3s;
    }
  }
  #grp4 {
    opacity: 1;
    transition: 0.1s all 0.3s;
    #oval1 {
      transform: scale(0) translate(30px, 15px);
      transform-origin: 0 0 0;
      transition: 0.5s transform 0.3s;
    }
    #oval2 {
      transform: scale(0) translate(40px, 50px);
      transform-origin: 0 0 0;
      transition: 1.5s transform 0.3s;
    }
  }
  #grp5 {
    opacity: 1;
    transition: 0.1s all 0.3s;
    #oval1 {
      transform: scale(0) translate(-10px, 20px);
      transform-origin: 0 0 0;
      transition: 0.5s transform 0.3s;
    }
    #oval2 {
      transform: scale(0) translate(-60px, 30px);
      transform-origin: 0 0 0;
      transition: 1.5s transform 0.3s;
    }
  }
  #grp6 {
    opacity: 1;
    transition: 0.1s all 0.3s;
    #oval1 {
      transform: scale(0) translate(-30px, 0px);
      transform-origin: 0 0 0;
      transition: 0.5s transform 0.3s;
    }
    #oval2 {
      transform: scale(0) translate(-60px, -5px);
      transform-origin: 0 0 0;
      transition: 1.5s transform 0.3s;
    }
  }
  #grp7 {
    opacity: 1;
    transition: 0.1s all 0.3s;
    #oval1 {
      transform: scale(0) translate(-30px, -15px);
      transform-origin: 0 0 0;
      transition: 0.5s transform 0.3s;
    }
    #oval2 {
      transform: scale(0) translate(-55px, -30px);
      transform-origin: 0 0 0;
      transition: 1.5s transform 0.3s;
    }
  }
  #grp2 {
    opacity: 1;
    transition: 0.1s opacity 0.3s;
  }
  #grp3 {
    opacity: 1;
    transition: 0.1s opacity 0.3s;
  }
  #grp4 {
    opacity: 1;
    transition: 0.1s opacity 0.3s;
  }
  #grp5 {
    opacity: 1;
    transition: 0.1s opacity 0.3s;
  }
  #grp6 {
    opacity: 1;
    transition: 0.1s opacity 0.3s;
  }
  #grp7 {
    opacity: 1;
    transition: 0.1s opacity 0.3s;
  }
}

@keyframes animateCircle {
  40% {
    transform: scale(10);
    opacity: 1;
    fill: #dd4688;
  }
  55% {
    transform: scale(11);
    opacity: 1;
    fill: #d46abf;
  }
  65% {
    transform: scale(12);
    opacity: 1;
    fill: #cc8ef5;
  }
  75% {
    transform: scale(13);
    opacity: 1;
    fill: transparent;
    stroke: #cc8ef5;
    stroke-width: 0.5;
  }
  85% {
    transform: scale(17);
    opacity: 1;
    fill: transparent;
    stroke: #cc8ef5;
    stroke-width: 0.2;
  }
  95% {
    transform: scale(18);
    opacity: 1;
    fill: transparent;
    stroke: #cc8ef5;
    stroke-width: 0.1;
  }
  100% {
    transform: scale(19);
    opacity: 1;
    fill: transparent;
    stroke: #cc8ef5;
    stroke-width: 0;
  }
}

@keyframes animateHeart {
  0% {
    transform: scale(0.2);
  }
  40% {
    transform: scale(1.2);
  }
  100% {
    transform: scale(1);
  }
}

@keyframes animateHeartOut {
  0% {
    transform: scale(1.4);
  }
  100% {
    transform: scale(1);
  }
}
</style>