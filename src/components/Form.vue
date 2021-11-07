<template>
  <div class="form">
    <form ref="form" class="my-form" @submit.prevent="print">
      <h3 class="full-width">Личные данные</h3>
      <div class="wrapper__last-name">
        <label for="last-name">Фамилия</label>
        <input name="last-name" type="text" class="last-name input" />
      </div>
      <div class="wrapper__first-name">
        <label for="first-name">Имя</label>
        <input name="first-name" type="text" class="first-name input" />
      </div>
      <div class="wrapper__patronymic">
        <label for="patronimyc">Отчество</label>
        <input name="patronymic" type="text" class="patronimyc input" />
      </div>
      <div class="birthday-date">
        <label for="birthday">Дата рождения</label>
        <input name="birthday-date" type="date" class="birthday input" />
      </div>
      <div class="wrapper__email full-width">
        <label for="email">E-mail</label>
        <input name="email" type="email" class="email input" />
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
      <!-- <input
          name="citizenship"
          list="citizenships"
          placeholder="Start typing..."
          v-model="selected_citizenship"
        />
        <datalist id="citizenships" class="citizenship__selector input">
          <option
            v-for="option in citizenship"
            :value="option.nationality"
            :key="option.id"
          >
            {{ option.nationality }}
          </option>
        </datalist> -->
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
            autocomplete="off"
            @focus="isDropdownCitOpen = true"
            v-model="selected_citizenship"
          />
          <img
            class="svg"
            role="img"
            style="width: 16px"
            src="https://www.svgrepo.com/show/7882/down-arrow.svg"
          />
        </div>
        <div v-if="isDropdownCitOpen" class="citizenship-selector__dropdown">
          <ul class="list">
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
          <input name="pass-seria" type="text" class="pass-seria input" />
        </div>
        <div class="wrapper__pass-num">
          <label for="pass-num">Номер паспорта</label>
          <input name="pass-num" type="text" class="pass-num input" />
        </div>
        <div class="wrapper__pass-date">
          <label for="pass-date">Дата выдачи</label>
          <input name="pass-date" type="date" class="pass-date input" />
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
            name="last-name-other"
            type="text"
            class="last-name-other input"
          />
        </div>
        <div class="wrapper__first-name-other">
          <label for="first-name-other">Имя на латинице</label>
          <input
            name="first-name-other"
            type="text"
            class="first-name-other input"
          />
        </div>
        <span class="full-width"
          >Иностранцы заполняют латинскими буквами. Например, Ivanov Ivan</span
        >
        <div class="wrapper__pass-num-other">
          <label for="pass-num-other">Номер паспорта</label>
          <input
            name="pass-num-other"
            type="text"
            class="pass-num-other input"
          />
        </div>
        <!-- <div class="wrapper__country">
          <label for="pass-country__selector">Страна выдачи</label>
          <select
            name="pass-country"
            class="pass-country__selector input"
            v-model="selected_country"
          >
            <option
              v-for="option in citizenship"
              :value="option.nationality"
              :key="option.uid"
            >
              {{ option.nationality }}
            </option>
          </select>
        </div> -->
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
                v-for="option in citizenship"
                :value="option.country"
                :key="option.uid"
                @click="chooseCountry"
              >
                {{ option.nationality }}
              </li>
            </ul>
          </div>
        </div>
        <!-- <div class="wrapper__pass-type">
          <label for="pass-type__selector">Тип паспорта</label>
          <select
            name="pass-type"
            class="pass-type__selector input"
            v-model="selected_pass_type"
          >
            <option
              v-for="option in pass_types"
              :value="option.type"
              :key="option.id"
            >
              {{ option.type }}
            </option>
          </select>
        </div> -->
          <div class="wrapper__passport-selector" v-click-outside="hidePassportTypes">
          <label for="passport_type">Тип паспорта</label>
          <br />

          <div class="input-container">
            <input
              id="passport_type"
              class="input"
              autocomplete="off"
              @focus="isDropdownTypesOpen = true"
              v-model="selected_pass_type"
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
              name="сhanged-name"
              class="answer"
              value="Нет"
              v-model="name_picked"
            />
            Нет
          </label>

          <label class="yes-label">
            <input
              type="radio"
              name="сhanged-name"
              class="answer"
              value="Да"
              v-model="name_picked"
            />
            Да
          </label>
        </div>
      </div>
      <div v-if="name_picked === 'Да'" class="changed-name">
        <div class="wrapper__new-last-name">
          <label for="new-last-name">Предыдущая фамилия</label>
          <input name="new-last-name" type="text" class="new-last-name input" />
        </div>
        <div class="wrapper__new-first-name">
          <label for="new-first-name">Предыдущее имя</label>
          <input
            name="new-first-name"
            type="text"
            class="new-first-name input"
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
// import debounce  from "../helpers/debounce.js";

export default {
  data() {
    return {
      form: null,
      isDropdownCitOpen: false,
      isDropdownCountryOpen: false,
      isDropdownTypesOpen: false,
      citizenship: citizenshipJson,
      country: citizenshipJson,
      pass_types: passTypesJson,
      selected_citizenship: '',
      selected_country: '',
      selected_pass_type: '',
      gender_picked: 'Мужской',
      name_picked: 'Да',
      debouncedSearchCitizenship: null,

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
    },
    getCitizenship(searchWord) {
      console.log("FETCH CITIZENSHIP EVENT: GET CITIZENSHIP FROM API", searchWord);

      this.citizenship = citizenshipJson.filter((cit) =>
        cit.title.includes(searchWord)
      );
    },
    chooseCitizenship(e) {
      this.selected_citizenship = e.target.innerText;
    },
    chooseCountry(e) {
      this.selected_country = e.target.innerText;
    },
    chooseType(e) {
      this.selected_pass_type = e.target.innerText;
    }
  },
    created() {
    // this.citizenship = citizenshipJson;
    // this.debouncedSearchSkills = debounce(this.getCitizenship, 2000);  
    },
  directives: {
    ClickOutside,
  },

};
</script>

<style lang="less" scoped>
.input-container {
  position: relative;
}
.svg {
  position: absolute;
  right: 4px;
  top: 16px;
}
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
  &__new-last-name {
    grid-column: 1 e("/") 4;
  }
  &__new-first-name {
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
.citizenship-selector{
  &__dropdown {
    position: relative;
  }
}
.country-selector{
  &__dropdown {
    position: relative;
  }
}
.types-selector{
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
</style>
