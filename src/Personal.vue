<template>
  <div id="personal" v-touch:swipe.left="slideHam" v-touch:swipe.right="slideHam2">
    <div class="bar">
      <div class="bar__back-arrow" v-on:click="slideHam2"></div>
      <h2 class="bar__title">Personalizuj</h2>
    </div>
    <div class="input-box">
      <form id="my-form">
        <div class="input-box__item">
          <label class="input-box__label" for>Wiek</label>
          <input class="input-box__input" type="number" placeholder="-" v-model="age" required />
        </div>
        <div class="input-box__item">
          <label class="input-box__label" for>Wzrost</label>
          <input class="input-box__input" type="number" placeholder="cm" v-model="height" required />
        </div>
        <div class="input-box__item">
          <label class="input-box__label" for>Płeć</label>
          <input type="radio" name="gender" id="maleId" value="male" />
          <label for="maleId" class="input-box__radio">Mężczyzna</label>
          <br />
          <input type="radio" name="gender" id="femaleId" value="female" />
          <label for="femaleId" class="input-box__radio">Kobieta</label>
          <br />
        </div>
        <div class="input-box__item" :on="click">
          <label class="input-box__label" for>Cel</label>
          <input
            class="input-box__input"
            type="number"
            placeholder="kg"
            v-model="weightGoal"
            required
          />
        </div>
        <div class="input-box__item">
          <label class="input-box__label" for>Aktywność</label>
          <select name class="input-box__select" v-model="activity">
            <option value="1.2">brak aktywności, praca siedząca</option>
            <option value="1.35">praca siedząca, 1-2 treningi w tygodniu</option>
            <option value="1.55">praca siedząca, treningi 3-4 razy w tygodniu</option>
            <option value="1.75">praca fizyczna, 3-4 treningi w tygodniu</option>
            <option value="2">zawodowy sportowiec, osoba codziennie trenująca</option>
          </select>
        </div>
        <button v-on:click="setData()" class="input-box__submit-btn" type="submit">Ustaw</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      height: null,
      sex: "",
      age: null,
      activity: null
    };
  },
  methods: {
    storeAge() {
      if (isNaN(this.age)) {
        alert("błędne dane");
      } else {
        this.$store.commit("storeAge", this.age);
      }
    },
    storeHeight() {
      if (isNaN(this.height)) {
        alert("błędne dane");
      } else {
        this.$store.commit("storeHeight", this.height);
      }
    },
    storeSex() {
      var male = document.getElementById("maleId");
      if (male.checked === true) {
        this.sex = "M";
      } else {
        this.sex = "K";
      }
      this.$store.commit("storeSex", this.sex);
    },
    storeWeightGoal() {
      if (isNaN(this.weightGoal)) {
        alert("błędne danexe");
      } else {
        console.log("weightGoal poszlo");
        this.$store.commit("storeWeightGoal", this.weightGoal);
      }
    },
    storeActivity() {
      if (this.activity === null) {
        this.activity = 1.2;
      } else {
        let parsedActivity = parseFloat(this.activity);
        this.$store.commit("storeActivity", parsedActivity);
      }
    },
    setData() {
      this.storeAge();
      this.storeHeight();
      this.storeSex();
      this.storeWeightGoal();
      this.storeActivity();
      localStorage.setItem("mojeInfo", JSON.stringify(this.personalInfo));
      // console.log("KKKK");
      // location.reload();
    },
    updateInfo() {
      this.$store.commit("updateInfo");
    },
    slideHam2() {
      this.$emit("slideHam2");
    },
    slideHam() {
      this.$emit("slideHam");
    },
    updateSexInput() {
      var male = document.getElementById("maleId");
      var female = document.getElementById("femaleId");
      if (this.sex === "K") {
        female.checked = true;
      } else {
        male.checked = true;
      }
    }
  },
  computed: {
    personalInfo() {
      return this.$store.state.personalInfo;
    }
  },
  created() {
    this.updateInfo();
    this.age = this.personalInfo.age;
    this.height = this.personalInfo.height;
    this.sex = this.personalInfo.sex;
    this.weightGoal = this.personalInfo.weightGoal;
    this.activity = this.personalInfo.activity;
  },
  mounted() {
    this.updateSexInput();
  }
};
</script>

<style scoped>
#personal {
  background: #fff;
  height: 100%;
  width: 100%;
  z-index: 555;
  position: absolute;
  top: 0;
}

.input-box {
  width: 100%;
  z-index: 5;
  overflow: hidden;
}

.bar__title {
  position: relative;
  padding: 0;
  font-size: 24px;
  line-height: 24px;
  font-weight: 600;
  top: 14px;
  padding: 0 14px;
}

.input-box form {
  padding: 20px 0px;
}

.input-box__item {
  color: rgba(0, 0, 0, 0.5);
}

.input-box__input {
  width: 100%;
  border: none;
  font-size: 17px;
  line-height: 48px;
  border-radius: 2px;
  padding-left: 20px;
  color: rgba(0, 0, 0, 0.5);
}

.input-box__input[type="text"] {
  text-transform: capitalize;
}

.input-box__select {
  border: none;
  width: 100%;
  padding: 0 16px;
  color: rgba(0, 0, 0, 0.5);
  height: 48px;
  line-height: 22px;
  font-size: 17px;
  background: #fff;
}

.input-box__label {
  color: #000;
  font-size: 17px;
  font-weight: 600;
  display: block;
  line-height: 28px;
  padding: 12px 0 0 20px;
  background: #f4f6f7;
}

.input-box__submit-btn {
  background: #388697;
  color: white;
  text-transform: capitalize;
  width: 157px;
  height: 40px;
  border: none;
  outline: none;
  border-radius: 2px;
  font-size: 18px;
  font-weight: bold;
  float: right;
  margin: 40px 20px;
}

input[type="radio"] {
  display: none;
}

.input-box__radio {
  font-size: 17px;
  line-height: 48px;
  display: inline-block;
  margin: 0 20px;
  transform: translateX(30px);
}

.input-box__radio:before,
.input-box__radio:after {
  content: "";
  display: block;
  position: absolute;
  box-sizing: border-box;
  width: 17px;
  height: 17px;
  border-radius: 20px;
  left: -30px;
  top: 50%;
  transform: translateY(-50%);
}

.input-box__radio:before {
  border: 4.5px solid #fff;
  background: transparent;
}

.input-box__radio:after {
  border: 2px solid #999999;
}

input[type="radio"]:checked + .input-box__radio:before {
  background: #388697;
}

input[type="radio"]:checked + .input-box__radio:after {
  border-color: #388697;
}

.bar {
  height: 50px;
  margin-bottom: 10px;
  display: flex;
}

.bar__back-arrow {
  height: 16px;
  width: 16px;
  box-sizing: border-box;
  border-top: 3px solid black;
  border-left: 3px solid black;
  border-radius: 3px;
  margin: 0 10px 0 20px;
  position: relative;
  transform: rotate(-45deg);
  top: 17px;
}
</style>

