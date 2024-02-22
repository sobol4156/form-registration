<template>
  <div class="customer-form">
    <h2>Форма создания Клиента</h2>

    <form @submit.prevent="submitForm">
      <!-- Основные атрибуты -->
      <div class="all_form">
        <div class="main_form">
          <div class="form-group">
            <div class="group-error">
              <label for="last-name">Фамилия*</label>

              <input
                v-model="formData.lastName"
                id="last-name"
                placeholder="Введите Фамилию"
                @blur="$v.formData.lastName.$touch()"
              />
              <div v-if="$v.formData.lastName.$error" class="error-message">
                <span v-if="!$v.formData.lastName.required"
                  >Обязательное поле</span
                >
                <span v-if="!$v.formData.lastName.alpha">Только кириллица</span>
              </div>
            </div>
          </div>

          <div class="form-group">
            <label for="first-name">Имя*</label>
            <input
              v-model="formData.firstName"
              id="first-name"
              placeholder="Введите Имя"
              @blur="$v.formData.firstName.$touch()"
            />
            <div v-if="$v.formData.firstName.$error" class="error-message">
              <span v-if="!$v.formData.firstName.required"
                >Обязательное поле</span
              >
              <span v-if="!$v.formData.firstName.alpha">Только кириллица</span>
            </div>
          </div>

          <div class="form-group">
            <label for="patronymic">Отчество</label>
            <input
              v-model="formData.patronymic"
              id="patronymic"
              @blur="$v.formData.patronymic.$touch()"
            />
            <div v-if="$v.formData.patronymic.$error" class="error-message">
              <span v-if="!$v.formData.patronymic.alpha">Только кириллица</span>
            </div>
          </div>

          <div class="form-group">
            <label for="dob">Дата рождения*</label>
            <input
              type="text"
              v-model="formData.dob"
              id="dob"
              placeholder="10.01.1990"
              @input="formatDob"
              @blur="$v.formData.dob.$touch()"
            />
            <div v-if="$v.formData.dob.$error" class="error-message">
              <span v-if="!$v.formData.dob.required">Обязательное поле</span>
              <span
                v-if="
                  !$v.formData.dob.customDobFormat && this.formData.dob !== ''
                "
                >Неверный формат</span
              >
            </div>
          </div>

          <div class="form-group">
            <label for="phone">Номер телефона*</label>
            <input
              type="tel"
              v-model="formData.phone"
              id="phone"
              placeholder="+7"
              @blur="$v.formData.phone.$touch()"
            />
            <div v-if="$v.formData.phone.$error" class="error-message">
              <span v-if="!$v.formData.phone.required">Обязательное поле</span>
              <span
                v-if="!$v.formData.phone.regex && this.formData.phone !== ''"
                >Некорректный формат номера телефона (ожидается +7 (999)
                668-61-03 или +79996686103)</span
              >
            </div>
          </div>

          <div class="form-group">
            <label>Пол</label>
            <select v-model="formData.gender">
              <option value="male">Мужской</option>
              <option value="female">Женский</option>
            </select>
          </div>

          <div class="form-group">
            <label for="customer-group"
              >Группа клиентов*
              <span style="font-size: 10px"
                >Вы можете использовать ctrl + left-mouse для выбора</span
              ></label
            >
            <select
              class="field_select"
              multiple
              v-model="formData.customerGroup"
              id="customer-group"
              @blur="$v.formData.customerGroup.$touch()"
            >
              <option value="VIP">VIP</option>
              <option value="problematic">Проблемные</option>
              <option value="OMS">ОМС</option>
            </select>
            <div class="selected">
              <span>
                <span v-if="this.formData.customerGroup.length > 0"
                  >Выбранные категории:
                </span>
                <span v-else>Ничего не выбрано</span>
                {{ this.formData.customerGroup + [] }}
              </span>
            </div>
            <div v-if="$v.formData.customerGroup.$error" class="error-message">
              <span v-if="!$v.formData.customerGroup.required"
                >Обязательное поле</span
              >
            </div>
          </div>

          <div class="form-group">
            <label for="attending-doctor">Лечащий врач</label>
            <select v-model="formData.attendingDoctor" id="attending-doctor">
              <option value="Ivanov">Иванов</option>
              <option value="Zaharov">Захаров</option>
              <option value="Chernysheva">Чернышева</option>
            </select>
          </div>

          <div class="">
            <input type="checkbox" v-model="formData.noSMS" id="no-sms" />
            <label for="no-sms">Не отправлять СМС</label>
          </div>
        </div>

        <!-- Адрес -->
        <div class="adresses">
          <h3>Адрес</h3>
          <div class="form-group">
            <label for="index">Индекс</label>
            <input v-model="formData.address.index" id="index" />
          </div>
          <div class="form-group">
            <label for="country">Страна</label>
            <input v-model="formData.address.country" id="country" />
          </div>
          <div class="form-group">
            <label for="region">Область</label>
            <input v-model="formData.address.region" id="region" />
          </div>
          <div class="form-group">
            <label for="city">Город*</label>
            <input
              v-model="formData.address.city"
              id="city"
              @blur="$v.formData.address.city.$touch()"
            />
            <div v-if="$v.formData.address.city.$error" class="error-message">
              <span v-if="!$v.formData.address.city.required"
                >Обязательное поле</span
              >
              <span v-if="!$v.formData.address.city.alpha"
                >Только кириллица</span
              >
            </div>
          </div>
          <div class="form-group">
            <label for="street">Улица</label>
            <input v-model="formData.address.street" id="street" />
          </div>
          <div class="form-group">
            <label for="house">Дом</label>
            <input v-model="formData.address.house" id="house" />
          </div>
        </div>

        <!-- Паспорт -->
        <div class="pasport">
          <h3>Паспорт</h3>
          <div class="form-group">
            <label for="document-type">Тип документа*</label>
            <select v-model="formData.passport.documentType" id="document-type">
              <option value="passport">Паспорт</option>
              <option value="birth-certificate">
                Свидетельство о рождении
              </option>
              <option value="driver-license">Вод. удостоверение</option>
            </select>
          </div>
          <div class="form-group">
            <label for="series">Серия</label>
            <input
              placeholder="0123"
              v-model="formData.passport.series"
              id="series"
            />
          </div>
          <div class="form-group">
            <label for="number">Номер</label>
            <input
              placeholder="456789"
              v-model="formData.passport.number"
              id="number"
            />
          </div>
          <div class="form-group">
            <label for="IssuedBy">Кем выдан</label>
            <input v-model="formData.passport.IssuedBy" id="IssuedBy" />
          </div>
          <div class="form-group">
            <label for="DateOfIssue">Дата выдачи*</label>
            <input
              type="text"
              v-model="formData.passport.DateOfIssue"
              id="DateOfIssue"
              placeholder="17.12.1990"
              @input="formatDateOfIssue"
              @blur="$v.formData.passport.DateOfIssue.$touch()"
            />
            <div
              v-if="$v.formData.passport.DateOfIssue.$error"
              class="error-message"
            >
              <span
                v-if="
                  !$v.formData.passport.DateOfIssue.required &&
                  this.formData.passport.DateOfIssue !== ''
                "
                >Обязательное поле</span
              >
              <span v-if="!$v.formData.passport.DateOfIssue.customDateFormat"
                >Неверный формат</span
              >
            </div>
          </div>
        </div>
      </div>

      <!-- Кнопка отправки формы -->
      <div class="form-group">
        <button
          type="submit"
          :class="{ 'disabled-button': $v.formData.$invalid }"
        >
          Создать Клиента
        </button>
      </div>
    </form>

    <div v-if="successMessage" class="success-message">
      {{ successMessage }}
    </div>
    <div v-if="errorMessage" class="error-message">
      {{ errorMessage }}
    </div>
  </div>
</template>

<script>
import { required, minLength } from "vuelidate/lib/validators";

export default {
  data() {
    return {
      formData: {
        lastName: "",
        firstName: "",
        patronymic: "",
        dob: "",
        phone: "+7",
        gender: "male",
        customerGroup: [],
        attendingDoctor: "",
        noSMS: false,
        address: {
          index: "",
          country: "",
          region: "",
          city: "",
          street: "",
          house: "",
        },
        passport: {
          documentType: "passport",
          series: "",
          number: "",
          IssuedBy: "",
          DateOfIssue: "",
        },
      },
      successMessage: "",
      errorMessage: "",
    };
  },
  validations: {
    formData: {
      lastName: { required, alpha: (val) => /^[а-яё]*$/i.test(val) },
      firstName: { required, alpha: (val) => /^[а-яё]*$/i.test(val) },
      patronymic: { alpha: (val) => /^[а-яё]*$/i.test(val), minLength },
      dob: {
        required,
        customDobFormat: function (value) {
          const regex = /^\d{2}\.\d{2}\.\d{4}$/;
          return regex.test(value);
        },
      },
      phone: {
        required,
        customPhoneValidator: function (value) {
          const regex = /^\+7 \(\d{3}\) \d{3}-\d{2}-\d{2}$|^\+7\d{10}$/;
          return regex.test(value);
        },
      },
      customerGroup: { required },
      attendingDoctor: { minLength },
      address: {
        index: { minLength },
        country: { minLength },
        region: { minLength },
        city: { required, alpha: (val) => /^[а-яё]*$/i.test(val) },
        street: { minLength },
        house: { minLength },
      },
      passport: {
        series: { minLength },
        number: { minLength },
        IssuedBy: { minLength },
        DateOfIssue: {
          required,
          customDateFormat: function (value) {
            const regex = /^\d{2}\.\d{2}\.\d{4}$/;
            return regex.test(value);
          },
        },
      },
    },
  },

  
  methods: {
    submitForm() {
      if (!this.$v.formData.$invalid) {
        this.errorMessage = "";
        this.successMessage = "Новый клиент успешно создан";
      } else {
        this.errorMessage = "Заполните недостающие поля";
      }
    },
    formatDob() {
      // Убираем все символы, кроме цифр
      let formattedValue = this.formData.dob.replace(/[^\d]/g, "");

      // Добавляем точки при вводе
      if (formattedValue.length >= 2) {
        formattedValue =
          formattedValue.slice(0, 2) + "." + formattedValue.slice(2);
      }
      if (formattedValue.length >= 5) {
        formattedValue =
          formattedValue.slice(0, 5) + "." + formattedValue.slice(5);
      }

      
      this.formData.dob = formattedValue;
    },
    formatDateOfIssue() {
  // Убираем все символы, кроме цифр
  let formattedValue = this.formData.passport.DateOfIssue.replace(/[^\d]/g, '');

  // Добавляем точки при вводе
  if (formattedValue.length >= 2) {
    formattedValue = formattedValue.slice(0, 2) + '.' + formattedValue.slice(2);
  }
  if (formattedValue.length >= 5) {
    formattedValue = formattedValue.slice(0, 5) + '.' + formattedValue.slice(5);
  }

  // Устанавливаем отформатированное значение
  this.formData.passport.DateOfIssue = formattedValue;
},
  },
};
</script>

<style scoped lang="sass">
body
  font-family: OpenSans

.disabled-button
  opacity: 0.6
  cursor: default

.customer-form
  max-width: 600px
  margin: 0 auto
  padding: 20px

.form-group
  margin-bottom: 15px

  label
    display: block
    margin-bottom: 5px

  input, select, button
    width: 100%
    padding: 8px
    box-sizing: border-box
    border: 2px solid #cdd6f3

  button
    background-color: #4caf50
    color: #fff
    cursor: pointer
    border: 1px solid #cdd6f3


  .error-message
    color: #d9534f
    padding: 0
    margin: 0
    font-size: 12px
    
  &.has-error
    input
      border: 1px solid red

.success-message
  margin-top: 20px
  background-color: #dff0d8
  padding: 10px
  border: 1px solid #d6e9c6
  color: #3c763d

.error-message
  margin-top: 20px
  background-color: #ffffff
  padding: 10px
  color: #d9534f

.field_select
  top: calc(100% - 2px)
  left: 0
  width: 100%
  border: 2px solid #cdd6f3
  border-bottom-right-radius: 2px
  border-bottom-left-radius: 2px
  box-sizing: border-box
  outline-color: #cdd6f3
  z-index: 6

  &[multiple]
    overflow-y: auto

  option
    display: block
    padding: 8px 16px
    width: calc(370px - 32px)
    cursor: pointer

  option:checked
    background-color: #eceff3

  option:hover
    background-color: #d5e8fb

.customerUse
  gap: 10px

.selected
  height: 32px
  text-align: start
  font-size: 24px
  border: 2px solid #cdd6f3

.group-error
</style>