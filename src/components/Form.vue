<template>
  <div class="form">
      <b-form @submit="onSubmit">
          <b-form-group
              label="Личные данные"
              label-size="lg"
              label-class="font-weight-bold pt-0">
              <b-form-row class="mb-2">
                  <b-col>
                      <label :for="'last-name'">Фамилия</label>
                      <b-form-input
                          id="last-name"
                          v-model="form.lastName"
                          :state="state.lastName"
                      ></b-form-input>
                  </b-col>
                  <b-col>
                      <label :for="'first-name'">Имя</label>
                      <b-form-input
                          id="first-name"
                          v-model="form.firstName"
                          :state="state.firstName"
                      ></b-form-input>
                  </b-col>
                  <b-col>
                      <label :for="'middle-name'">Отчество</label>
                      <b-form-input
                          id="middle-name"
                          v-model="form.middleName"
                          :state="state.middleName"
                      ></b-form-input>
                  </b-col>
              </b-form-row>

              <b-form-row class="mb-2">
                  <b-col cols="4">
                      <label :for="'birth-date'">Дата рождения</label>
                      <b-form-datepicker
                          id="birth-date"
                          class="mb-2"
                          v-model="form.birthDate"
                          :state="state.birthDate"
                          placeholder="дд.мм.гггг"
                          locale="ru"
                          :date-format-options="{ year: 'numeric', month: 'numeric', day: 'numeric' }"
                      ></b-form-datepicker>
                  </b-col>
              </b-form-row>

              <b-form-row class="mb-2">
                  <b-col cols="4">
                      <label :for="'email'">E-mail</label>
                      <b-form-input
                          id="email"
                          v-model="form.email"
                          :state="state.email"
                          type="email"
                      ></b-form-input>
                  </b-col>
              </b-form-row>

              <b-form-row class="mb-2">
                  <b-col cols="3">
                      <b-form-group label="Пол" v-slot="{ ariaDescribedby }">
                          <b-form-radio-group
                              v-model="form.gender"
                              :options="genders"
                              :aria-describedby="ariaDescribedby"
                          >
                              <b-form-invalid-feedback
                                  :state="state.gender"
                              >Пожалуйста, выберите один из вариантов</b-form-invalid-feedback>
                          </b-form-radio-group>
                      </b-form-group>
                  </b-col>
              </b-form-row>
          </b-form-group>

          <b-form-group
              label="Паспортные данные"
              label-size="lg"
              label-class="font-weight-bold pt-0">
              <b-form-row class="mb-2">
                  <b-col cols="4">
                      <label :for="'citizenship'">Гражданство</label>
                      <b-form-select
                          v-model="form.citizenship"
                          :options="citizenships"
                          :state="state.citizenship"
                          value-field="id"
                          text-field="nationality"
                      ></b-form-select>
                  </b-col>
              </b-form-row>

              <b-form-row class="mb-2" v-if="isRussianCitizenship">
                  <b-col>
                      <label :for="'passport-series'">Серия паспорта</label>
                      <b-form-input
                          id="passport-series"
                          v-model="form.passportSeries"
                          :state="state.passportSeries"
                      ></b-form-input>
                  </b-col>
                  <b-col>
                      <label :for="'passport-id'">Номер паспорта</label>
                      <b-form-input
                          id="passport-id"
                          v-model="form.passportId"
                          :state="state.passportId"
                      ></b-form-input>
                  </b-col>
                  <b-col>
                      <label :for="'date-of-issue'">Дата выдачи</label>
                      <b-form-datepicker
                          id="date-of-issue"
                          class="mb-2"
                          v-model="form.dateOfIssue"
                          :state="state.dateOfIssue"
                          placeholder="дд.мм.гггг"
                          locale="ru"
                          :date-format-options="{ year: 'numeric', month: 'numeric', day: 'numeric' }"
                      ></b-form-datepicker>
                  </b-col>
              </b-form-row>

              <b-form-group class="mb-2" v-else>
                  <b-form-row class="mb-2">
                      <b-col cols="6">
                          <label :for="'last-name-latin'">Фамилия на латинице</label>
                          <b-form-input
                              id="last-name-latin"
                              v-model="form.lastNameLatin"
                              :state="state.lastNameLatin"
                          ></b-form-input>
                      </b-col>
                      <b-col cols="6">
                          <label :for="'first-name-latin'">Имя на латинице</label>
                          <b-form-input
                              id="first-name-latin"
                              v-model="form.firstNameLatin"
                              :state="state.firstNameLatin"
                          ></b-form-input>
                      </b-col>
                  </b-form-row>
                  <b-form-text>Иностранцы заполняют латинскими буквами. Например, Ivanov Ivan.</b-form-text>

                  <b-form-row class="mb-2">
                      <b-col cols="3">
                          <label :for="'passport-id'">Номер паспорта</label>
                          <b-form-input
                              id="passport-id"
                              v-model="form.passportId"
                              :state="state.passportId"
                          ></b-form-input>
                      </b-col>
                      <b-col cols="3">
                          <label :for="'date-of-issue'">Страна выдачи</label>
                          <b-form-select
                              v-model="form.countryOfIssue"
                              :options="citizenships"
                              :state="state.countryOfIssue"
                              value-field="id"
                              text-field="nationality"
                          ></b-form-select>
                      </b-col>
                      <b-col cols="6">
                          <label :for="'date-of-issue'">Тип паспорта</label>
                          <b-form-select
                              v-model="form.passportType"
                              :options="passportTypes"
                              :state="state.passportType"
                              value-field="id"
                              text-field="type"
                          ></b-form-select>
                      </b-col>
                  </b-form-row>
              </b-form-group>

              <b-form-row class="mb-2">
                  <b-col cols="3">
                      <b-form-group label="Меняли ли фамилию или имя" v-slot="{ ariaDescribedby }">
                          <b-form-radio-group
                              v-model="form.firstNameOrLastNameChanged"
                              :options="yesNo"
                              :aria-describedby="ariaDescribedby"
                          >
                              <b-form-invalid-feedback
                                  :state="state.firstNameOrLastNameChanged"
                              >Пожалуйста, выберите один из вариантов</b-form-invalid-feedback>
                          </b-form-radio-group>
                      </b-form-group>
                  </b-col>
              </b-form-row>

              <b-form-row class="mb-2" v-if="form.firstNameOrLastNameChanged">
                  <b-col>
                      <label :for="'last-name-changed'">Фамилия</label>
                      <b-form-input
                          id="last-name-changed"
                          v-model="form.lastNameChanged"
                          :state="state.lastNameChanged"
                      ></b-form-input>
                  </b-col>
                  <b-col>
                      <label :for="'first-name-changed'">Имя</label>
                      <b-form-input
                          id="first-name-changed"
                          v-model="form.firstNameChanged"
                          :state="state.firstNameChanged"
                      ></b-form-input>
                  </b-col>
              </b-form-row>
          </b-form-group>

          <b-button type="submit" variant="primary">Отправить</b-button>
      </b-form>
      <b-card class="mt-3" header="Form Data">
          <pre class="m-0">{{ form }}</pre>
      </b-card>
  </div>
</template>

<script>
import citizenships from '../assets/data/citizenships.json';
import passportTypes from '../assets/data/passport-types.json';

export default {
    data() {
        return {
            form: {
                firstName: '',
                lastName: '',
                middleName: '',
                birthDate: '',
                email: '',
                gender: null,
                citizenship: null,
                passportSeries: '',
                passportId: '',
                dateOfIssue: '',
                lastNameLatin: '',
                firstNameLatin: '',
                countryOfIssue: null,
                passportType: null,
                firstNameOrLastNameChanged: null,
                lastNameChanged: '',
                firstNameChanged: '',
            },
            state: {
                firstName: null,
                lastName: null,
                middleName: null,
                birthDate: null,
                email: null,
                gender: null,
                citizenship: null,
                passportSeries: null,
                passportId: null,
                dateOfIssue: null,
                lastNameLatin: null,
                firstNameLatin: null,
                countryOfIssue: null,
                passportType: null,
                firstNameOrLastNameChanged: null,
                lastNameChanged: null,
                firstNameChanged: null,
            },
            genders: [
                {text: 'Мужской', value: 'male'},
                {text: 'Женский', value: 'female'}
            ],
            citizenships: [
                {id: null, nationality: 'Пожалуйста выберите гражданство'},
            ],
            passportTypes: [
                {id: null, type: 'Пожалуйста выберите тип паспорта'},
            ],
            yesNo: [
                {value: true, text: 'Да'},
                {value: false, text: 'Нет'},
            ],
            russianCitizenshipId: undefined,
        };
    },
    created() {
        this.citizenships.push(...citizenships);
        this.passportTypes.push(...passportTypes);

        const russianCitizenship = citizenships.find(citizenship => citizenship.nationality.toLowerCase() === 'russia');
        this.russianCitizenshipId = russianCitizenship ? russianCitizenship.id : undefined;
    },
    computed: {
        isRussianCitizenship() {
            return this.form.citizenship === this.russianCitizenshipId;
        },
    },
    methods: {
        onSubmit(event) {
            event.preventDefault()
            this.checkForm()
            console.log('Form Data', this.form)
        },
        checkForm() {
            const {
                firstNameOrLastNameChanged,
                birthDate,
                gender,
                citizenship,
                countryOfIssue,
                passportType,
                email,
                passportSeries,
                passportId,
                dateOfIssue
            } = this.form;

            // Фамилия, Имя, Отчество, Предыдущая Фамилия, Предыдущее Имя- только русские буквы
            const cyrillicFields = ['firstName', 'lastName', 'middleName'];
            const latinFields = [];
            if (firstNameOrLastNameChanged) {
                cyrillicFields.push('lastNameChanged', 'firstNameChanged')
            } else {
                latinFields.push('lastNameLatin', 'firstNameLatin')
            }

            cyrillicFields.forEach(field => {
                this.state[field] = this.onlyCyrillic(this.form[field])
            })

            if (latinFields.length) {
                latinFields.forEach(field => {
                    this.state[field] = this.onlyLatin(this.form[field])
                })
            }

            this.state.birthDate = this.dateNotInPast(birthDate)
            this.state.email = this.validEmail(email)

            this.state.dateOfIssue = (dateOfIssue !== null);
            this.state.gender = (gender !== null);
            this.state.firstNameOrLastNameChanged = (firstNameOrLastNameChanged !== null);
            this.state.citizenship = (citizenship !== null);
            this.state.countryOfIssue = (countryOfIssue !== null);
            this.state.passportType = (passportType !== null);

            if (this.isRussianCitizenship) {
                this.state.passportSeries = this.fourDigits(passportSeries)
            }

            this.state.passportId = this.sixDigits(passportId)
        },
        onlyCyrillic(field) {
            // Фамилия, Имя, Отчество, Предыдущая Фамилия, Предыдущее Имя
            // матчим "русскую" кириллицу. see: https://stackoverflow.com/a/52257128/13172702
            const re = /^[\u0401\u0451\u0410-\u044f]+$/;
            return re.test(field);
        },
        onlyLatin(field) {
            // Фамилия на латинице и Имя на латинице - только английские буквы
            const re = /^[a-zA-Z]+$/;
            return re.test(field);
        },
        fourDigits(field) {
            // Серия паспорта - 4 цифры
            const re = /^\d{4}$/;
            return re.test(field);
        },
        sixDigits(field) {
            // Номер паспорта - 6 цифр для гражданства Россия и любое значение для других гражданств
            if (!this.isRussianCitizenship) {
                return !!field;
            }

            const re = /^\d{6}$/;
            return re.test(field);
        },
        validEmail(email) {
            // E-mail - валидный почтовый адрес
            const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
            return re.test(email);
        },
        dateNotInPast(date) {
            // yyyy-mm-dd
            // Дата Рождения - валидная дата, не позже сегодняшнего числа
            // строка дана в формате ISO, например "2014-03-07", parse() будет предполагать, что часовой пояс равен UTC
            const parsedDate = Date.parse(date);
            if (!date) {
                return false;
            } else if (isNaN(parsedDate)) {
                return false;
            }

            // часовой пояс равен UTC
            const currentDate = new Date()
                .setUTCHours(0,0,0,0);
            return (parsedDate >= currentDate);
        }
    },
};
</script>

<style scoped>
.form {
}
</style>
