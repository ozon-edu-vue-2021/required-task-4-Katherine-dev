<template>
  <div class="form">
    <form ref="form" class="my-form" @submit.prevent="validate">
      <h3 class="full-width">Личные данные</h3>
      <div class="wrapper__last-name">
        <label for="last-name">Фамилия</label>
        <input
          name="last_name"
          type="text"
          ref="last_name"
          class="last-name input"
          required
        />
      </div>
      <div class="wrapper__first-name">
        <label for="first-name">Имя</label>
        <input
          name="first_name"
          type="text"
          ref="first_name"
          class="first-name input"
          required
        />
      </div>
      <div class="wrapper__patronymic">
        <label for="patronimyc">Отчество</label>
        <input
          name="patronimyc"
          type="text"
          ref="patronimyc"
          class="patronimyc input"
        />
      </div>
      <div class="birthday-date">
        <label for="birthday">Дата рождения</label>
        <input
          name="birthday_date"
          ref="birthday_date"
          type="date"
          class="birthday input"
          required
        />
      </div>
      <div class="wrapper__email full-width">
        <label for="email">E-mail</label>
        <input
          name="email"
          type="text"
          ref="email"
          class="email input"
          placeholder="test-email@mail.com"
          required
        />
      </div>
      <div class="wrapper__gender">
        <span class="gender">Пол</span>
        <br />
        <div class="gender__title">
          <label>
            <input
              type="radio"
              class="answer"
              name="gender"
              value="Мужской"
              v-model="gender_picked"
              checked
            />
            Мужской
          </label>

          <label>
            <input
              type="radio"
              class="answer"
              name="gender"
              value="Женский"
              v-model="gender_picked"
            />
            Женский
          </label>
        </div>
      </div>
      <h3 class="full-width">Паспортные данные</h3>
      <div
        class="wrapper__citizenship-selector"
        v-click-outside="hideCitizenships"
      >
        <label for="citizenship"> Гражданство </label>
        <br />
        <div class="input-container">
          <input
            id="citizenship"
            class="input"
            v-model="searchCitizenship"
            autocomplete="off"
            @focus="isDropdownCitOpen = true"
            required
          />
          <img
            class="svg"
            role="img"
            style="width: 16px"
            src="https://www.svgrepo.com/show/7882/down-arrow.svg"
          />
        </div>
        <div v-if="isDropdownCitOpen" class="citizenship-selector__dropdown">
          <ul class="list" v-if="citizenship.length">
            <li
              class="li"
              v-for="option in citizenship"
              :value="option.citizenship"
              :key="option.id"
              @click="chooseCitizenship"
            >
              {{ option.nationality }}
            </li>
          </ul>
          <div v-else class="empty">Ничего не найдено</div>
        </div>
      </div>
      <div
        class="citizenship__rus"
        v-if="
          selected_citizenship === 'Russia' && selected_citizenship.length !== 0
        "
      >
        <div class="wrapper__pass-seria">
          <label for="pass-seria">Серия паспорта</label>
          <input
            name="pass_seria"
            ref="pass_seria"
            type="text"
            class="pass-seria input"
            required
          />
        </div>
        <div class="wrapper__pass-num">
          <label for="pass-num">Номер паспорта</label>
          <input
            name="pass_num"
            ref="pass_num"
            type="text"
            class="pass-num input"
            required
          />
        </div>
        <div class="wrapper__pass-date">
          <label for="pass-date">Дата выдачи</label>
          <input name="pass_date" type="date" class="pass-date input" required/>
        </div>
      </div>
      <div
        class="citizenship__other"
        v-if="
          selected_citizenship !== 'Russia' && selected_citizenship.length !== 0
        "
      >
        <div class="wrapper__last-name-other">
          <label for="last-name-other">Фамилия на латинице</label>
          <input
            name="last_name_other"
            ref="last_name_other"
            type="text"
            class="last-name-other input"
          />
        </div>
        <div class="wrapper__first-name-other">
          <label for="first-name-other">Имя на латинице</label>
          <input
            name="first_name_other"
            ref="first_name_other"
            type="text"
            class="first-name-other input"
            required
          />
        </div>
        <span class="full-width"
          >Иностранцы заполняют латинскими буквами. Например, Ivanov Ivan</span
        >
        <div class="wrapper__pass-num-other">
          <label for="pass-num-other">Номер паспорта</label>
          <input
            name="pass_num_other"
            type="text"
            ref="pass_num_other"
            class="pass-num-other input"
            required
          />
        </div>
        <div class="wrapper__country-selector" v-click-outside="hideCountries">
          <label for="country">Страна выдачи</label>
          <br />

          <div class="input-container">
            <input
              id="country"
              class="input"
              autocomplete="off"
              @focus="isDropdownCountryOpen = true"
              v-model="selected_country"
              required
            />
            <img
              class="svg"
              role="img"
              style="width: 16px"
              src="https://www.svgrepo.com/show/7882/down-arrow.svg"
            />
          </div>

          <div v-if="isDropdownCountryOpen" class="country-selector__dropdown">
            <ul class="list">
              <li
                class="li"
                v-for="option in countries"
                :value="option.country"
                :key="option.uid"
                @click="chooseCountry"
              >
                {{ option.nationality }}
              </li>
            </ul>
          </div>
        </div>
        <div
          class="wrapper__passport-selector"
          v-click-outside="hidePassportTypes"
        >
          <label for="passport_type">Тип паспорта</label>
          <br />

          <div class="input-container">
            <input
              id="passport_type"
              class="input"
              autocomplete="off"
              @focus="isDropdownTypesOpen = true"
              v-model="selected_pass_type"
              required
            />
            <img
              class="svg"
              role="img"
              style="width: 16px"
              src="https://www.svgrepo.com/show/7882/down-arrow.svg"
            />
          </div>

          <div v-if="isDropdownTypesOpen" class="types-selector__dropdown">
            <ul class="list pass-list">
              <li
                class="li"
                v-for="option in pass_types"
                :value="option.type"
                :key="option.id"
                @click="chooseType"
              >
                {{ option.type }}
              </li>
            </ul>
          </div>
        </div>
      </div>

      <div class="wrapper__changed-name">
        <span class="сhanged-name">Меняли ли вы фамилию или имя?</span>
        <br />
        <div class="changed-name__answer">
          <label>
            <input
              type="radio"
              name="сhanged_name"
              class="answer"
              value="Нет"
              v-model="name_picked"
              checked
            />
            Нет
          </label>

          <label class="yes-label">
            <input
              type="radio"
              name="сhanged_name"
              class="answer"
              value="Да"
              v-model="name_picked"
            />
            Да
          </label>
        </div>
      </div>
      <div v-if="name_picked === 'Да'" class="changed-name">
        <div class="wrapper__old-last-name">
          <label for="old-last-name">Предыдущая фамилия</label>
          <input
            name="old_last_name"
            type="text"
            ref="old_last_name"
            class="old-last-name input"
            required
          />
        </div>
        <div class="wrapper__old-first-name">
          <label for="old-first-name">Предыдущее имя</label>
          <input
            name="old_first_name"
            type="text"
            ref="old_first_name"
            class="old-first-name input"
            required
          />
        </div>
      </div>

      <div class="wrapper__send-button">
        <button class="send-button" type="submit">Отправить</button>
      </div>
    </form>
  </div>
</template>

<script>
import citizenshipJson from "../assets/data/citizenships.json"
import passTypesJson from "../assets/data/passport-types.json"
import ClickOutside from "vue-click-outside";
import { debounce } from "../helpers/debounce.js";

const RUS_REG_EXP = /^[А-Яа-яЁё]+/;
// Отчество может отсутствовать
const PATR_REG_EXP = /^[А-Яа-яЁё]*/;
const EMAIL_REG_EXP = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
const DATE_REG_EXP = /^\d{4}-(0?[1-9]|1[012])-(0?[1-9]|[12][0-9]|3[01])$/;
const PASS_SER_REG_EXP =/^\d{4}$/;
const PASS_NUM_RUS_REG_EXP =/^\d{6}$/;
const ENG_REG_EXP = /^[A-Za-z]+/;

export default {
  data() {
    return {
      isDropdownCitOpen: false,
      isDropdownCountryOpen: false,
      isDropdownTypesOpen: false,
      citizenship: citizenshipJson,
      countries: citizenshipJson,
      pass_types: passTypesJson,
      selected_citizenship: '',
      selected_country: '',
      selected_pass_type: '',
      gender_picked: 'Мужской',
      name_picked: 'Нет',
      debouncedSearchCitizenship: null,
      searchCitizenship: ''

    };
  },
  methods: {
    hideCitizenships() {
      this.isDropdownCitOpen = false;
    },
    hideCountries() {
      this.isDropdownCountryOpen = false;
    },
    hidePassportTypes() {
      this.isDropdownTypesOpen = false;
    },
    print() {
      const data = new FormData(this.$refs.form);
      const value = Object.fromEntries(data);
      console.log(value);
      this.$refs.form.reset();
    },
    getCitizenship(searchWord) {
      console.log("FETCH CITIZENSHIP EVENT: GET CITIZENSHIP FROM API", searchWord);

      this.citizenship = citizenshipJson.filter((cit) => {
        return cit.nationality.toLowerCase().includes(searchWord.toLowerCase()) }
      );
    },
    chooseCitizenship(e) {
      this.searchCitizenship = e.target.innerText;
      this.selected_citizenship = e.target.innerText;
      if (this.selected_citizenship === 'Russia') {
        this.selected_country = '';
        this.selected_pass_type = '';
      }
      this.hideCitizenships();
    },
    chooseCountry(e) {
      this.selected_country = e.target.innerText;
      this.hideCountries();
    },
    chooseType(e) {
      this.selected_pass_type = e.target.innerText;
      this.hidePassportTypes();
    },
    onRusInput() {
      let valid = true;
      const data = new FormData(this.$refs.form);
      const value = Object.fromEntries(data);
      if (!RUS_REG_EXP.test(value.last_name)) {
        this.$refs.last_name.classList.add('invalid');
        alert("Проверьте правильность ввода Фамилии. Можно вводить только русские буквы.");
        valid = false;
      } else {
        this.$refs.last_name.classList.remove('invalid');
      }
      if (!RUS_REG_EXP.test(value.first_name)) {
        this.$refs.first_name.classList.add('invalid');
        alert("Проверьте правильность ввода Имени. Можно вводить только русские буквы.");
        valid = false;
      } else {
        this.$refs.first_name.classList.remove('invalid');
      }
      if (!PATR_REG_EXP.test(value.patronimyc)) {
        this.$refs.patronimyc.classList.add('invalid');
        alert("Проверьте правильность ввода Отчества. Можно вводить только русские буквы.");
        valid = false;
      } else {
        this.$refs.patronimyc.classList.remove('invalid');
      }
      if (this.name_picked === 'Да') {
        if (!RUS_REG_EXP.test(value.old_last_name)) {
          this.$refs?.old_last_name?.classList.add('invalid');
          alert("Проверьте правильность ввода Предыдущей фамилии. Можно вводить только русские буквы.");
          valid = false;
        } else {
          this.$refs?.old_last_name?.classList.remove('invalid');
        }
        if (!RUS_REG_EXP.test(value.old_first_name)) {
          this.$refs?.old_first_name?.classList.add('invalid');
          alert("Проверьте правильность ввода Предыдущего имени. Можно вводить только русские буквы.");
          valid = false;
        } else {
          this.$refs?.old_first_name?.classList.remove('invalid');
        }
      }
      return valid;
    },
    validateBirthDate() {
      const data = new FormData(this.$refs.form);
      const value = Object.fromEntries(data);
      let today = new Date().toISOString().substring(0, 10);
      if (!DATE_REG_EXP.test(value.birthday_date) || value.birthday_date > today || value.birthday_date < '1900-01-01') {
        alert("Проверьте правильность ввода Даты рождения");
        this.$refs.birthday_date.classList.add('invalid');
        return false;
      } else {
        this.$refs.birthday_date.classList.remove('invalid');
      }
      
      return true;
    },
    validateEmail() {
      const data = new FormData(this.$refs.form);
      const value = Object.fromEntries(data);

      if (!EMAIL_REG_EXP.test(value.email)) {
        alert("Проверьте правильность ввода email");
        this.$refs.email.classList.add('invalid');
        return false;
      } else {
        this.$refs.email.classList.remove('invalid');
      }

      return true;
    },
    validatePassport() {
      let valid = true;
      const data = new FormData(this.$refs.form);
      const value = Object.fromEntries(data);
      if (this.selected_citizenship === 'Russia') {
        if (!PASS_SER_REG_EXP.test(value.pass_seria)) {
          alert("Проверьте правильность ввода Серии паспорта. Она должна состоять из четрых цифр.");
          this.$refs.pass_seria.classList.add('invalid');
          valid = false;
        } else {
          this.$refs.pass_seria.classList.remove('invalid');
        }
        if (!PASS_NUM_RUS_REG_EXP.test(value.pass_num)) {
          alert("Проверьте правильность ввода Номера паспорта. Он должен состоять из шести цифр.");
          this.$refs.pass_num.classList.add('invalid');
          valid = false;
        } else {
          this.$refs.pass_num.classList.remove('invalid');
        }
      } else {
        if (!ENG_REG_EXP.test(value.last_name_other)) {
          alert("Проверьте правильность ввода Фамилии на латинице");
          this.$refs.last_name_other.classList.add('invalid');
          valid = false;
        } else {
          this.$refs.last_name_other.classList.remove('invalid');
        }
        if (!ENG_REG_EXP.test(value.first_name_other)) {
          alert("Проверьте правильность ввода Имени на латинице");
          this.$refs.first_name_other.classList.add('invalid');
          valid = false;
        } else {
          this.$refs.first_name_other.classList.remove('invalid');
        }
      }

      return valid;
    },
    validate() { 
      const isValidRus = this.onRusInput();
      const isValidDate = this.validateBirthDate();
      const isValidEmail = this.validateEmail();
      const isValidPassport = this.validatePassport();
      
      const isValid = isValidRus && isValidDate && isValidEmail && isValidPassport;

      if (isValid) {
        this.print();
        this.name_picked = "Нет";
        this.gender_picked = "Мужской";
        this.selected_citizenship = '';
        this.searchCitizenship = '';
        this.selected_country = '';
        this.selected_pass_type = '';
      }
    }
  },
  created() {
    this.citizenship = citizenshipJson;
    this.debouncedSearchCitizenship = debounce(this.getCitizenship, 1000);
  },
  watch: {
    searchCitizenship(newValue) {
      this.debouncedSearchCitizenship(newValue);
    },
  },
  directives: {
    ClickOutside,
  },

};
</script>

<style lang="less" scoped>
.my-form {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr;
  grid-gap: 16px;
}
.wrapper {
  &__last-name {
    grid-column: 1 e("/") 3;
  }
  &__first-name {
    grid-column: 3 e("/") 5;
  }
  &__patronymic {
    grid-column: 5 e("/") 7;
  }
  &__gender {
    grid-column: 1 e("/") 6;
  }
  &__citizenship-selector {
    grid-column: 1 e("/") 4;
  }
  &__pass-seria {
    grid-column: 1 e("/") 2;
  }
  &__pass-num {
    grid-column: 2 e("/") 4;
  }
  &__pass-date {
    grid-column: 4 e("/") 6;
  }
  &__changed-name {
    grid-column: 1 e("/") 7;
  }
  &__old-last-name {
    grid-column: 1 e("/") 4;
  }
  &__old-first-name {
    grid-column: 4 e("/") 7;
  }
  &__last-name-other {
    grid-column: 1 e("/") 4;
  }
  &__first-name-other {
    grid-column: 4 e("/") 7;
  }
  &__pass-num-other {
    grid-column: 1 e("/") 2;
  }
  &__country-selector {
    grid-column: 2 e("/") 4;
  }
  &__passport-selector {
    grid-column: 4 e("/") 7;
  }
  &__send-button {
    grid-column: 6 e("/") 7;
  }
}
.input-container {
  position: relative;
}
.svg {
  position: absolute;
  right: 4px;
  top: 16px;
}
.gender {
  &__title {
    margin-top: 8px;
  }
}
.changed-name {
  &__answer {
    margin-top: 8px;
  }
}
.yes-label {
  margin-left: 8px;
}
.gender,
.changed_name {
  font-weight: 700;
}

.answer {
  width: auto;
}

.full-width {
  grid-column: 1 e("/") 7;
}
.birthday-date {
  grid-column: 1 e("/") 4;
}
.citizenship {
  &__selector {
    width: 100%;
  }
  &__rus,
  &__other {
    display: contents;
  }
}
.list {
  list-style-type: none;
  padding: 0;
  margin: 0;
  border: 1px solid grey;
  height: 100px;
  overflow-y: auto;
  position: absolute;
  background-color: white;
  width: 100%;
}
.pass-list {
  height: 55px;
  overflow: hidden;
}
.citizenship-selector {
  &__dropdown {
    position: relative;
  }
}
.empty {
  margin-top: 8px;
}
.country-selector {
  &__dropdown {
    position: relative;
  }
}
.types-selector {
  &__dropdown {
    position: relative;
  }
}
.li {
  padding: 4px;
}
.li:hover {
  background-color: lightgrey;
}
.changed-name {
  display: contents;
}
.input {
  width: 100%;
  border: 1px solid LightGray;
  padding: 8px;
  margin-top: 4px;
  font-size: 16px;
}
.pass-date {
  padding: 6px;
}
.send-button {
  width: 100%;
  background: #2962ff;
  color: #fff;
  padding: 12px;
  border-radius: 3px;
  border: 0;
  font-size: 16px;
}
.invalid {
  transition: 0.28s;
  border: 1.5px solid rgba(226, 27, 16, 0.5);
}
</style>
