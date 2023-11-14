<template>
  <form @submit.prevent="handleSubmit" novalidate>
    <label>Email: </label>
    <input v-model="email" type="email" />
    <div v-if="errorMessageE" class="error">{{ errorMessageE }}</div>

    <label>Password</label>
    <input
      style="font-size: 15px"
      v-model="password"
      @keyup="handleKeyPass"
      maxlength="13"
      @blur="changeVisibility"
      type="password"
    />
    <div class="strengthBarD">
      <span v-show="visibilityMeter" class="strengthBarS"></span>

      <i
        v-show="xIcon && !visibilityMeter"
        style="color: red"
        class="fa-solid fa-xmark fa-bounce"
      ></i>
      <i
        v-show="cIcon && !visibilityMeter"
        style="color: green"
        class="fa-solid fa-circle-check"
      ></i>
    </div>
    <div v-if="errorMessageP" class="errorP">{{ errorMessageP }}</div>
    <label>Role:</label>

    <select v-model="role">
      <option value="" selected hidden>Choose here</option>
      <option value="webDeveloper">Web Developer</option>
      <option value="webDesigner">Web Designer</option>
      <option value="softwareDeveloper">Software Developer</option>
    </select>
    <div v-if="errorMessageSe" class="error">{{ errorMessageSe }}</div>

    <label>Skills</label>
    <input
      placeholder="Write your skills one by one, following a comma."
      v-model="tempSkill"
      @keyup="funkcija"
      type="text"
    />

    <div v-for="skill in skills" :key="skill" class="pill">
      <span @click="clickSkill(skill)"> {{ skill }}</span>
    </div>
    <div v-if="errorMessageSk" class="error">{{ errorMessageSk }}</div>
    <div class="submit">
      <button>Create an Account</button>
    </div>
  </form>
</template>

<script>
export default {
  data() {
    return {
      email: "",
      password: "",
      role: "",
      tempSkill: "",
      skills: [],
      errorMessageP: "",
      errorMessageE: "",
      errorMessageSe: "",
      errorMessageSk: "",
      result: "",
      visibilityMeter: true,
      weakPassword: /^[a-z]{6,}$/,
      strongPassword: /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).{6,}$/,
      emailRegex: /^.{5,}@([\w-]+\.)+[\w-]{2,4}$/,
      xIcon: false,
      cIcon: false,
    };
  },
  methods: {
    funkcija(e) {
      if (
        e.key === "," &&
        this.tempSkill.length > 1 &&
        this.tempSkill[0] != ","
      ) {
        this.tempSkill = this.tempSkill.slice(0, this.tempSkill.length - 1);
        this.skills.push(this.tempSkill);
        this.tempSkill = "";
      }
    },
    clickSkill(skill) {
      this.skills.splice(this.skills.indexOf(skill), 1);
    },

    handleKeyPass(event) {
      this.visibilityMeter = true;
      this.errorMessageP = "";
      const bar = document.querySelector(".strengthBarS");
      if (this.password.length == 0) {
        bar.style.width = "0";
      } else if (this.password.length < 6) {
        bar.style.width = "25%";
        bar.style.backgroundColor = "red";
      } else if (this.weakPassword.test(this.password)) {
        bar.style.width = "60%";
        bar.style.backgroundColor = "brown";
      } else if (this.strongPassword.test(this.password)) {
        bar.style.width = "100%";
        bar.style.backgroundColor = "green";
      }
    },
    changeVisibility() {
      this.visibilityMeter = false;

      if (this.strongPassword.test(this.password)) {
        this.xIcon = false;
        this.cIcon = true;
      } else if (this.password.length >= 1) {
        if (this.password.length < 6) {
          this.errorMessageP = "Password length must be at least 6 characters!";
        } else if (this.weakPassword.test(this.password)) {
          this.errorMessageP = "Weak Password!";
        }

        this.xIcon = true;
        this.cIcon = false;
      }
    },

    handleSubmit() {
      this.result =
        "Email: " + this.email + "\nRole: " + this.role + "\nSkills: ";

      this.skills.forEach((element, index) => {
        if (index != this.skills.length - 1) {
          this.result += element + ", ";
        } else {
          this.result += element + ".";
        }
      });

      if (this.password.length < 6) {
        this.errorMessageP = "Password length must be at least 6 characters!";
      } else if (this.weakPassword.test(this.password)) {
        this.errorMessageP = "Weak Password!";
      } else {
        this.errorMessageP = "";
      }

      if (this.email.length === 0) {
        this.errorMessageE = "Please insert your mail";
      } else if (!this.emailRegex.test(this.email)) {
        this.errorMessageE = "This doesnt satisfy email requirements!";
      } else {
        this.errorMessageE = "";
      }

      if (this.role == "") {
        this.errorMessageSe = "You must choose a role";
      } else {
        this.errorMessageSe = "";
      }

      if (this.skills.length === 0) {
        this.errorMessageSk = "You must insert at least one skill";
      } else {
        this.errorMessageSk = "";
      }

      if (
        this.strongPassword.test(this.password) &&
        this.emailRegex.test(this.email) &&
        this.role && this.skills.length>0n
      ) {
        alert(this.result);
        this.errorMessageE = "";
        this.errorMessageP = "";
        this.errorMessageSe = "";
      }
    },
  },
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
body {
  background: linear-gradient(to right, #b97309, #9b610a);
}

form {
  max-width: 370px;
  margin: 30px auto;
  background: white;
  text-align: left;
  padding: 40px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

label {
  color: #aaa;
  display: inline-block;
  margin: 25px 0 15px;
  font-size: 0.6em;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}

input,
select {
  display: block;
  padding: 10px 6px;
  width: 100%;
  color: #555;
  border-radius: 10px;
}

.pill {
  display: inline-block;
  margin: 20px 10px 0 0;
  padding: 6px 12px;
  background: #eee;
  border-radius: 20px;
  font-size: 12px;
  letter-spacing: 1px;
  font-weight: bold;
  color: #777;
  cursor: pointer;
}

button {
  background: #0b6dff;
  border: 0;
  padding: 10px 20px;
  margin-top: 20px;
  color: white;
  border-radius: 20px;
}

.submit {
  text-align: center;
}

.error {
  color: red;
  margin-top: 10px;
  font-size: 0.8em;
  font-weight: bold;
}

.errorP {
  color: red;
  font-size: 0.8em;
  font-weight: bold;
  position: relative;
  top: -15px;
}

.strengthBarS {
  width: 50%;
  display: block;
  position: relative;
  height: 100%;
}

.strengthBarD {
  background-color: #fffefe;
  height: 15px;
  position: relative;
  margin-top: 2px;
  border-radius: 0 0 5px 5px;
}

i {
  font-size: 16px;
  position: relative;
  top: 0;
  padding-right: 2.5px;
  float: right;
}



@media screen and (max-width: 367px) {
    h1 {
        font-size: 20px;
    }

    #app {
       max-width: 250px;
      margin:auto;
       }
}

@media screen and (min-width: 367px) {
    h1 {
        font-size: 40px;
    }
}
</style>