<template>
  <header>
    <div class="headerDiv">
      <img src="../assets/logo.png" alt="logo" />
      <a href="javascript:void(0);" class="icon" @click="myFunction">
        <i class="fas fa-bars"></i
      ></a>
      <!-- Font Awesome is designed to be used with inline elements, and we recommend sticking with a consistent HTML element to reference them by in your project-->
      <div class="navDiv" id="nav">
        <div class="nav">
          <router-link to="/"
            ><div class="menu">
              <i class="fas fa-home"></i> Home
            </div></router-link
          >
          <router-link to="/dictionary/start">
            <div class="menu">
              <i class="fas fa-search"></i> Dictionary
            </div></router-link
          >
          <router-link to="/browse">
            <div class="menu">
              <i class="fas fa-book-open"></i> Browse A-Z
            </div></router-link
          >
          <router-link to="/about"
            ><div class="menu">
              <i class="fas fa-comment"></i> About &amp; Contacts
            </div></router-link
          >
          <router-link to="/profile" v-if="user.email != null"
            ><div class="menu">
              <i class="fas fa-user"></i> Profile
            </div></router-link
          >
        </div>
        <div class="logDetails">
          <button
            class="primaryButton"
            tabindex="0"
            type="Button"
            @click="logout"
            v-if="user.email != null"
          >
            <span class="buttonLabel">Logout</span>
          </button>
          <button
            class="primaryButton"
            tabindex="0"
            type="Button"
            @click="goTo('login')"
            v-if="user.email == null"
          >
            <span class="buttonLabel">Login</span>
          </button>
          &nbsp;&nbsp;
          <button
            class="primaryButton"
            tabindex="0"
            type="Button"
            @click="goTo('signUp')"
            v-if="user.email == null"
          >
            <span class="buttonLabel">Sign up</span>
          </button>
        </div>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  //Function to get the token generated for the user
  computed: {
    token: {
      get: function () {
        return sessionStorage.getItem("token");
      },
      set: function () {},
    },
    user: {
      get: function () {
        return this.$store.state.userService.user;
      },
      set: function () {},
    },
  },
  //This function will be excecuted when the screen resized and loaded
  mounted() {
    window.onresize = this.resize;
  },
  //Class name will be changed as per the screen size
  methods: {
    resize() {
      var x = document.getElementById("nav");
      x.className = "navDiv";
    },
    //Class name toggled as per the scree size
    myFunction() {
      var x = document.getElementById("nav");
      if (x.className === "navDiv") {
        x.className += " responsive";
      } else {
        x.className = "navDiv";
      }
    },
    //Method called on login and signup click
    goTo(event) {
      if (event === "signUp") {
        this.$router.push("/signUp");
      } else if (event === "login") {
        this.$router.push("/login");
      } else {
        this.$confirm({
          auth: false,
          message: "Invalid Event received",
          button: {
            no: "Ok",
          },
        });
      }
    },
    logout() {
      sessionStorage.removeItem("token");
      const userDetails = {
        email: this.user.email,
        password: this.user.password,
      };
      this.$store.dispatch("logout", userDetails).then(() => {
        const error = this.$store.state.userService.error;
        if (error != "") {
          this.$confirm({
            auth: false,
            message: error,
            button: {
              no: "Ok",
            },
          });
        } else {
          this.$router.push("/login");
        }
      });
    },
  },
};
</script>

<style scoped lang="scss">
@import "../style/common";
header {
  position: sticky;
  top: 0%;
  color: #fff;
  display: flex;
  box-sizing: border-box;
  flex-shrink: 0;
  flex-direction: column;
  transition: box-shadow 300ms cubic-bezier(0.4, 0, 0.2, 1) 0ms;
  background-color: #1d2a57;
  border-bottom: 30px solid #fff;
}
.headerDiv {
  display: flex;
  position: relative;
  align-items: center;
  padding-left: 24px;
  padding-right: 24px;
  color: #fff;
}
.navDiv {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  width: 100%;
  padding-top: 8px;
  color: #13222d;
}
.nav {
  display: flex;
  justify-content: flex-start;
  margin-left: 20%;
  overflow: hidden;
}
.nav a {
  color: white;
  margin-right: 20px;
  text-decoration: none;
  margin-right: 20px;
  background-color: transparent;
  float: left;
  display: block;
}
.nav a.router-link-exact-active {
  color: #ec4b43;
  text-decoration: none;
}
.nav a:hover {
  color: #25d366;
  text-decoration: none;
}
.icon {
  display: none;
  color: #fff;
}
img {
  vertical-align: middle;
  border-style: none;
  height: 80px;
}
.logDetails {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
}

label {
  color: #fff;
  display: inline-block;
  margin: 15px 0 10px;
  text-transform: uppercase;
}

@media screen and (max-width: 800px) {
  .navDiv .nav {
    display: none;
  }
  .navDiv .logDetails {
    display: none;
  }
  .icon {
    float: right;
    display: block;
  }
}
@media screen and (max-width: 800px) {
  .navDiv.navDiv.responsive {
    top: 0;
    margin-top: 50px;
    right: 0;
    height: 90%;
    position: fixed;
    width: 35%;
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    flex-direction: column;
    transition: box-shadow 300ms cubic-bezier(0.4, 0, 0.2, 1) 0ms;
    background-color: #1d2a57;
  }
  .icon {
    position: absolute;
    top: 1;
    right: 0;
    font-size: 25px;
    margin-right: 20px;
  }
  .navDiv.responsive .nav {
    margin-left: 15px;
    display: flex;
    flex-direction: column;
    margin-top: 10px;
    flex-grow: 1;
    text-align: right;
  }
  .navDiv.responsive .nav a {
    display: block;
    text-align: left;
    margin-bottom: 20px;
  }
  .navDiv.responsive .nav i {
    margin-right: 10px;
  }
  .navDiv.responsive .logDetails {
    display: flex;
    flex-direction: column;
    text-align: left;
    position: relative;
    align-items: center;
    margin-bottom: 10px;
    margin-right: 10px;
  }
  .navDiv.responsive .menu {
    display: flex;
    flex-direction: row;
  }
}
</style>
