<script setup>
const MONTHS = [
    "Jan.",
    "Feb.",
    "Mär.",
    "Apr.",
    "Mai",
    "Jun.",
    "Jul.",
    "Aug.",
    "Sep.",
    "Okt.",
    "Nov",
    "Dez.",
];
const START_YEAR = 1920;
const CURRENT_YEAR = new Date().getFullYear();

import { Form, Field, FieldArray, ErrorMessage, configure } from "vee-validate";
import * as yup from "yup";
import { onMounted, ref } from "vue";
import flatpickr from "flatpickr";
import { German } from "flatpickr/dist/l10n/de.js";
import "flatpickr/dist/themes/light.css";

import countries from "./countries.json";

// disabling validation after changes and loss of focus
configure({
    validateOnBlur: false,
    validateOnChange: false,
});

const form = ref(null);
let id = 2;
// initial data fields of form
const formData = ref({
    travel_period: "",
    number_adults: undefined,
    childrens: [
        {
            id: 0,
            name: "",
            datebirth_date: undefined,
            datebirth_month: "",
            datebirth_year: undefined,
        },
        {
            id: 1,
            name: "",
            datebirth_date: undefined,
            datebirth_month: "",
            datebirth_year: undefined,
        },
    ],
    first_name: "",
    last_name: "",
    gender: "",
    email: "",
    country: "",
    zip: "",
    city: "",
    street: "",
    phone_number: "",
    questions_wishes: "",
});

// regular expression for email verification
const emailRegExp = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;

// creating a validation scheme based on the yup library
const schema = yup.object().shape({
    travel_period: yup
        .string()
        .required("Füllen Sie dieses Feld aus.")
        .min(25)
        .max(25),
    number_adults: yup
        .number()
        .moreThan(0)
        .required("Geben Sie eine positive Zahl ein.")
        .typeError("Geben Sie eine positive Zahl ein."),
    childrens: yup.array().of(
        yup.object({
            name: yup.string().required("Geben Sie eine Name ein."),
            datebirth_date: yup
                .number()
                .required("Füllen Sie ein Tag aus.")
                .typeError("Füllen Sie ein Tag aus."),
            datebirth_month: yup
                .number()
                .required("Füllen Sie ein Monat aus.")
                .typeError("Füllen Sie ein Monat aus."),
            datebirth_year: yup
                .number()
                .required("Füllen Sie ein Jahr aus.")
                .typeError("Füllen Sie ein Jahr aus."),
        }),
    ),
    first_name: yup.string().required("Geben Sie einen gültigen Vornamen ein."),
    last_name: yup.string().required("Geben Sie einen gültigen Nachnamen ein."),
    gender: yup.string().required("Füllen Sie dieses Feld aus."),
    email: yup
        .string()
        .email("Geben Sie eine gültige E-Mail-Adresse ein.")
        .required("Geben Sie eine gültige E-Mail-Adresse ein.")
        .matches(emailRegExp, "Geben Sie eine gültige E-Mail-Adresse ein."),
    country: yup.string().required("Füllen Sie dieses Feld aus."),
    zip: yup.string().required("Geben Sie einen gültigen PLZ ein."),
    city: yup.string().required("Geben Sie eine gültige Stadt ein."),
    street: yup.string().required("Geben Sie eine gültige Straße ein."),
    phone_number: yup
        .string()
        .test("phone_number", "Phone number is not valid", (str) => {
            if (!str) {
                return true;
            }
            return /^\+?\d[\d{6,15},\s,-]*$/.test(str);
        }),
});

// function to check for empty object
const isObjectEmpty = (objectName) => {
    return (
        objectName &&
        Object.keys(objectName).length === 0 &&
        objectName.constructor === Object
    );
};

let flatpickrEl = null;
// initialize the calendar when the component is mounted
onMounted(() => {
    let schowMonths = 2;
    if (window.innerWidth < 650) {
        schowMonths = 1;
    }

    flatpickrEl = flatpickr(".flatpickr", {
        mode: "range",
        minDate: "today",
        locale: German,
        showMonths: schowMonths, //need change for resize window
        wrap: true,
    });
});

const resetForm = () => {
    form.value.resetForm();
    flatpickrEl.clear();
};

function onSubmitForm(values) {
    console.log(values);
    resetForm();
}

function onInvalidSubmit({ values, errors, results }) {
    // console.log(values); // current form values
    // console.log(errors); // a map of field names and their first error message
    // console.log(results); // a detailed map of field names and their validation results

    document.getElementById("btnSubmit")?.scrollIntoView();
}

// adding fields for a child
function addChild(formData) {
    formData?.childrens?.push({
        id: id,
        name: "",
        datebirth_date: undefined,
        datebirth_month: "",
        datebirth_year: undefined,
    });
    id++;
}
// remove last child from array
function removeChild(formData) {
    formData?.childrens?.pop();
}
</script>

<template>
    <div class="form-traver-wrap">
        <!-- creating a form using the vee-validate library -->
        <Form
            ref="form"
            :validation-schema="schema"
            :initial-values="formData"
            @submit="onSubmitForm"
            @invalid-submit="onInvalidSubmit"
        >
            <!-- creating the first field group -->
            <div class="form-group">
                <div class="form-group-title sansita-swashed-regular">
                    Ihre Urlaubsdaten
                </div>
                <div class="form-group-container">
                    <div class="form-row">
                        <label class="form-row-label" for="travelperiod"
                            >Reisezeitraum *</label
                        >
                        <!-- creating a field calendar using the vee-validate, flatpickr libraries -->
                        <div class="flatpickr form-row-container">
                            <Field
                                type="text"
                                id="travelperiod"
                                name="travel_period"
                                v-model="formData.travel_period"
                                :class="
                                    form?.errors?.travel_period
                                        ? 'form-input-error'
                                        : ''
                                "
                                minLength="25"
                                maxLength="25"
                                required
                                data-input
                            />
                            <a class="input-button" title="toggle" data-toggle>
                                <svg
                                    version="1.0"
                                    id="Layer_1"
                                    xmlns="http://www.w3.org/2000/svg"
                                    xmlns:xlink="http://www.w3.org/1999/xlink"
                                    width="20px"
                                    height="20px"
                                    viewBox="0 0 64 64"
                                    enable-background="new 0 0 64 64"
                                    xml:space="preserve"
                                >
                                    <g>
                                        <path
                                            fill="#231F20"
                                            d="M11,54h6c0.553,0,1-0.447,1-1v-5c0-0.553-0.447-1-1-1h-6c-0.553,0-1,0.447-1,1v5C10,53.553,10.447,54,11,54
		z M12,49h4v3h-4V49z"
                                        />
                                        <path
                                            fill="#231F20"
                                            d="M23,54h6c0.553,0,1-0.447,1-1v-5c0-0.553-0.447-1-1-1h-6c-0.553,0-1,0.447-1,1v5C22,53.553,22.447,54,23,54
		z M24,49h4v3h-4V49z"
                                        />
                                        <path
                                            fill="#231F20"
                                            d="M35,54h6c0.553,0,1-0.447,1-1v-5c0-0.553-0.447-1-1-1h-6c-0.553,0-1,0.447-1,1v5C34,53.553,34.447,54,35,54
		z M36,49h4v3h-4V49z"
                                        />
                                        <path
                                            fill="#231F20"
                                            d="M11,43h6c0.553,0,1-0.447,1-1v-5c0-0.553-0.447-1-1-1h-6c-0.553,0-1,0.447-1,1v5C10,42.553,10.447,43,11,43
		z M12,38h4v3h-4V38z"
                                        />
                                        <path
                                            fill="#231F20"
                                            d="M23,43h6c0.553,0,1-0.447,1-1v-5c0-0.553-0.447-1-1-1h-6c-0.553,0-1,0.447-1,1v5C22,42.553,22.447,43,23,43
		z M24,38h4v3h-4V38z"
                                        />
                                        <path
                                            fill="#231F20"
                                            d="M35,43h6c0.553,0,1-0.447,1-1v-5c0-0.553-0.447-1-1-1h-6c-0.553,0-1,0.447-1,1v5C34,42.553,34.447,43,35,43
		z M36,38h4v3h-4V38z"
                                        />
                                        <path
                                            fill="#231F20"
                                            d="M47,43h6c0.553,0,1-0.447,1-1v-5c0-0.553-0.447-1-1-1h-6c-0.553,0-1,0.447-1,1v5C46,42.553,46.447,43,47,43
		z M48,38h4v3h-4V38z"
                                        />
                                        <path
                                            fill="#231F20"
                                            d="M11,32h6c0.553,0,1-0.447,1-1v-5c0-0.553-0.447-1-1-1h-6c-0.553,0-1,0.447-1,1v5C10,31.553,10.447,32,11,32
		z M12,27h4v3h-4V27z"
                                        />
                                        <path
                                            fill="#231F20"
                                            d="M23,32h6c0.553,0,1-0.447,1-1v-5c0-0.553-0.447-1-1-1h-6c-0.553,0-1,0.447-1,1v5C22,31.553,22.447,32,23,32
		z M24,27h4v3h-4V27z"
                                        />
                                        <path
                                            fill="#231F20"
                                            d="M35,32h6c0.553,0,1-0.447,1-1v-5c0-0.553-0.447-1-1-1h-6c-0.553,0-1,0.447-1,1v5C34,31.553,34.447,32,35,32
		z M36,27h4v3h-4V27z"
                                        />
                                        <path
                                            fill="#231F20"
                                            d="M47,32h6c0.553,0,1-0.447,1-1v-5c0-0.553-0.447-1-1-1h-6c-0.553,0-1,0.447-1,1v5C46,31.553,46.447,32,47,32
		z M48,27h4v3h-4V27z"
                                        />
                                        <path
                                            fill="#231F20"
                                            d="M60,4h-7V3c0-1.657-1.343-3-3-3s-3,1.343-3,3v1H17V3c0-1.657-1.343-3-3-3s-3,1.343-3,3v1H4
		C1.789,4,0,5.789,0,8v52c0,2.211,1.789,4,4,4h56c2.211,0,4-1.789,4-4V8C64,5.789,62.211,4,60,4z M49,3c0-0.553,0.447-1,1-1
		s1,0.447,1,1v3v4c0,0.553-0.447,1-1,1s-1-0.447-1-1V6V3z M13,3c0-0.553,0.447-1,1-1s1,0.447,1,1v3v4c0,0.553-0.447,1-1,1
		s-1-0.447-1-1V6V3z M62,60c0,1.104-0.896,2-2,2H4c-1.104,0-2-0.896-2-2V17h60V60z M62,15H2V8c0-1.104,0.896-2,2-2h7v4
		c0,1.657,1.343,3,3,3s3-1.343,3-3V6h30v4c0,1.657,1.343,3,3,3s3-1.343,3-3V6h7c1.104,0,2,0.896,2,2V15z"
                                        />
                                    </g>
                                </svg>
                            </a>
                            <!-- creating a field for dispaly a text error -->
                            <ErrorMessage name="travel_period" />
                        </div>
                    </div>
                    <div class="form-row">
                        <label class="form-row-label" for="numberadults"
                            >Anzahl Erwachsene *</label
                        >
                        <!-- creating a field text using the vee-validate library -->
                        <div class="form-row-container">
                            <Field
                                type="text"
                                id="numberadults"
                                name="number_adults"
                                placeholder=""
                                v-model.lazy="formData.number_adults"
                                :class="
                                    form?.errors?.number_adults
                                        ? 'form-input-error'
                                        : ''
                                "
                                required
                                min="1"
                                pattern="\d+"
                            />
                            <!-- creating a field for dispaly a text error -->
                            <ErrorMessage name="number_adults" />
                        </div>
                    </div>
                </div>
            </div>
            <div class="form-group">
                <div class="form-group-title">Kinder</div>
                <div class="form-group-subtitle">
                    Als Familienspezialist ist es uns wichtig, Ihnen ein
                    maßgeschneidertes Angebot zu übermitteln. Bitte geben Sie
                    uns daher den Vornamen und Das Alter Ihrer Kinder/Ihres
                    Kindes an.
                </div>
                <!-- creating dynamic fields from array fields children using the vee-validate library -->
                <FieldArray name="childrens">
                    <div
                        class="form-group-container"
                        v-for="(children, idx) in formData.childrens"
                        :key="children.id"
                    >
                        <div class="form-row">
                            <label
                                class="form-row-label"
                                :for="`childrens[${idx}].name`"
                                >Name des Kindes *</label
                            >
                            <div class="form-row-container">
                                <!-- creating text field using the vee-validate library -->
                                <Field
                                    type="text"
                                    :id="`childrens[${idx}].name`"
                                    :name="`childrens[${idx}].name`"
                                    v-model.lazy="formData.childrens[idx].name"
                                    :class="
                                        form?.errors?.[`childrens[${idx}].name`]
                                            ? 'form-input-error'
                                            : ''
                                    "
                                    required
                                />
                                <!-- creating a field for dispaly a text error -->
                                <ErrorMessage
                                    :name="`childrens[${idx}].name`"
                                />
                            </div>
                        </div>
                        <div class="form-row">
                            <label
                                class="form-row-label"
                                :for="`childrens[${idx}].datebirth_date`"
                                >Geburtstag *</label
                            >
                            <div class="form-row-container">
                                <div class="form-row-date-container">
                                    <div class="form-row-date-field-wrap">
                                        <!-- creating select field using the vee-validate library -->
                                        <Field
                                            as="select"
                                            :id="`childrens[${idx}].datebirth_date`"
                                            :name="`childrens[${idx}].datebirth_date`"
                                            v-model.lazy="
                                                formData.childrens[idx]
                                                    .datebirth_date
                                            "
                                            :class="
                                                form?.errors?.[
                                                    `childrens[${idx}].datebirth_date`
                                                ]
                                                    ? 'form-input-error'
                                                    : ''
                                            "
                                            required
                                        >
                                            <option value="" disabled>
                                                Tag
                                            </option>
                                            <option
                                                v-for="n in 31"
                                                :key="n"
                                                :value="n"
                                            >
                                                {{ n }}
                                            </option>
                                        </Field>
                                        <!-- creating a field for dispaly a text error -->
                                        <ErrorMessage
                                            :name="`childrens[${idx}].datebirth_date`"
                                        />
                                    </div>
                                    <div class="form-row-date-field-wrap">
                                        <!-- creating select field using the vee-validate library -->
                                        <Field
                                            as="select"
                                            :name="`childrens[${idx}].datebirth_month`"
                                            v-model.lazy="
                                                formData.childrens[idx]
                                                    .datebirth_month
                                            "
                                            :class="
                                                form?.errors?.[
                                                    `childrens[${idx}].datebirth_month`
                                                ]
                                                    ? 'form-input-error'
                                                    : ''
                                            "
                                            required
                                        >
                                            <option :key="0" value="" disabled>
                                                Monat
                                            </option>
                                            <option
                                                v-for="(month, index) in MONTHS"
                                                :key="index"
                                                :value="index"
                                            >
                                                {{ month }}
                                            </option>
                                        </Field>
                                        <!-- creating a field for dispaly a text error -->
                                        <ErrorMessage
                                            :name="`childrens[${idx}].datebirth_month`"
                                        />
                                    </div>
                                    <div class="form-row-date-field-wrap">
                                        <!-- creating select field using the vee-validate library -->
                                        <Field
                                            as="select"
                                            :name="`childrens[${idx}].datebirth_year`"
                                            v-model.lazy="
                                                formData.childrens[idx]
                                                    .datebirth_year
                                            "
                                            :class="
                                                form?.errors?.[
                                                    `childrens[${idx}].datebirth_year`
                                                ]
                                                    ? 'form-input-error'
                                                    : ''
                                            "
                                            required
                                        >
                                            <option value="" disabled>
                                                Jahr
                                            </option>
                                            <option
                                                v-for="year in CURRENT_YEAR -
                                                START_YEAR +
                                                1"
                                                :key="CURRENT_YEAR - year + 1"
                                                :value="CURRENT_YEAR - year + 1"
                                            >
                                                {{ CURRENT_YEAR - year + 1 }}
                                            </option>
                                        </Field>
                                        <!-- creating a field for dispaly a text error -->
                                        <ErrorMessage
                                            :name="`childrens[${idx}].datebirth_year`"
                                        />
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </FieldArray>
                <!-- creating buttons for add/remove child  -->
                <div class="form-row-kinder-links-container">
                    <button
                        @click="addChild(formData)"
                        type="button"
                        class="btn-increase"
                    >
                        Kind hinzufügen
                    </button>
                    <span>&#8594;</span>
                    <button
                        @click="removeChild(formData)"
                        type="button"
                        class="btn-decrease"
                    >
                        Kind entfernen
                    </button>
                </div>
            </div>
            <div class="form-group">
                <div class="form-group-title">Ihre Kontaktdaten</div>
                <div class="form-group-container">
                    <div class="form-row">
                        <label class="form-row-label" for="firstname"
                            >Vorname *</label
                        >
                        <div class="form-row-container">
                            <!-- creating text field using the vee-validate library -->
                            <Field
                                type="text"
                                id="firstname"
                                name="first_name"
                                v-model="formData.first_name"
                                :class="
                                    form?.errors?.first_name
                                        ? 'form-input-error'
                                        : ''
                                "
                                required
                            />
                            <!-- creating a field for dispaly a text error -->
                            <ErrorMessage name="first_name" />
                        </div>
                    </div>
                    <div class="form-row">
                        <label class="form-row-label" for="lastname"
                            >Nachname *</label
                        >
                        <div class="form-row-container">
                            <!-- creating text field using the vee-validate library -->
                            <Field
                                type="text"
                                id="lastname"
                                name="last_name"
                                v-model="formData.last_name"
                                :class="
                                    form?.errors?.last_name
                                        ? 'form-input-error'
                                        : ''
                                "
                                required
                            />
                            <!-- creating a field for dispaly a text error -->
                            <ErrorMessage name="last_name" />
                        </div>
                    </div>
                    <div class="form-row">
                        <label class="form-row-label" for="gender"
                            >Geschlecht *</label
                        >
                        <div class="form-row-container">
                            <!-- creating select field using the vee-validate library -->
                            <Field
                                as="select"
                                id="gender"
                                name="gender"
                                v-model="formData.gender"
                                :class="
                                    form?.errors?.gender
                                        ? 'form-input-error'
                                        : ''
                                "
                                required
                            >
                                <option value="" disabled>Auswählen</option>
                                <option value="men">Männlich</option>
                                <option value="women">Weiblich</option>
                            </Field>
                            <!-- creating a field for dispaly a text error -->
                            <ErrorMessage name="gender" />
                        </div>
                    </div>
                    <div class="form-row">
                        <label class="form-row-label" for="email"
                            >E-Mail *</label
                        >
                        <div class="form-row-container">
                            <!-- creating email field using the vee-validate library -->
                            <Field
                                type="email"
                                id="email"
                                name="email"
                                v-model.lazy="formData.email"
                                :class="
                                    form?.errors?.email
                                        ? 'form-input-error'
                                        : ''
                                "
                                required
                            />
                            <!-- creating a field for dispaly a text error -->
                            <ErrorMessage name="email" />
                        </div>
                    </div>
                    <div class="form-row">
                        <label class="form-row-label" for="country"
                            >Land *</label
                        >
                        <div class="form-row-container">
                            <!-- creating select field using the vee-validate library -->
                            <Field
                                as="select"
                                id="country"
                                name="country"
                                v-model.lazy="formData.country"
                                :class="
                                    form?.errors?.country
                                        ? 'form-input-error'
                                        : ''
                                "
                                required
                            >
                                <option value="" disabled>Auswählen</option>
                                <option key="DE" value="Duetschland">
                                    Duetschland
                                </option>
                                <option key="AT" value="Österreich">
                                    Österreich
                                </option>
                                <option key="CH" value="Duetschland">
                                    Schweiz
                                </option>
                                <option value="" disabled>
                                    ----------------------------------------
                                </option>
                                <option
                                    v-for="(country, index) in countries"
                                    :key="index"
                                    :value="country"
                                >
                                    {{ country }}
                                </option>
                            </Field>
                            <!-- creating a field for dispaly a text error -->
                            <ErrorMessage name="country" />
                        </div>
                    </div>
                    <div class="form-row">
                        <label class="form-row-label" for="zip">PLZ *</label>
                        <div class="form-row-container">
                            <!-- creating text field using the vee-validate library -->
                            <Field
                                type="text"
                                id="zip"
                                name="zip"
                                v-model.lazy="formData.zip"
                                :class="
                                    form?.errors?.zip ? 'form-input-error' : ''
                                "
                                required
                            />
                            <!-- creating a field for dispaly a text error -->
                            <ErrorMessage name="zip" />
                        </div>
                    </div>
                    <div class="form-row">
                        <label class="form-row-label" for="city">Stadt *</label>
                        <div class="form-row-container">
                            <!-- creating text field using the vee-validate library -->
                            <Field
                                type="text"
                                id="city"
                                name="city"
                                v-model.lazy="formData.city"
                                :class="
                                    form?.errors?.city ? 'form-input-error' : ''
                                "
                                required
                            />
                            <!-- creating a field for dispaly a text error -->
                            <ErrorMessage name="city" />
                        </div>
                    </div>
                    <div class="form-row">
                        <label class="form-row-label" for="street"
                            >Straße *</label
                        >
                        <div class="form-row-container">
                            <!-- creating text field using the vee-validate library -->
                            <Field
                                type="text"
                                id="street"
                                name="street"
                                v-model.lazy="formData.street"
                                :class="
                                    form?.errors?.street
                                        ? 'form-input-error'
                                        : ''
                                "
                                required
                            />
                            <!-- creating a field for dispaly a text error -->
                            <ErrorMessage name="street" />
                        </div>
                    </div>
                    <div class="form-row">
                        <label class="form-row-label" for="phone"
                            >Telefon</label
                        >
                        <div class="form-row-container">
                            <!-- creating phone field using the vee-validate library -->
                            <Field
                                type="tel"
                                id="phone"
                                name="phone_number"
                                v-model.lazy="formData.phone_number"
                                :class="
                                    form?.errors?.phone_number
                                        ? 'form-input-error'
                                        : ''
                                "
                            />
                            <!-- creating a field for dispaly a text error -->
                            <ErrorMessage name="phone_number" />
                        </div>
                    </div>
                    <div>{{ formData }}</div>
                    <div class="form-row">
                        <label class="form-row-label" for="questions_wishes"
                            >Fragen oder Wünsche</label
                        >
                        <div class="form-row-container">
                            <!-- creating textarea field using the vee-validate library -->
                            <Field
                                as="textarea"
                                id="questions_wishes"
                                name="questions_wishes"
                                v-model.lazy="formData.questions_wishes"
                                rows="4"
                            />
                        </div>
                    </div>
                </div>
            </div>
            <!-- display error block if object form.errors is not empty -->
            <div
                v-if="!isObjectEmpty(form?.errors)"
                class="form-error-container"
            >
                <div class="form-error-text">
                    Beim Senden des Formulars ist ein Fehler aufgetreten!
                </div>
                <div class="form-error-tipp">
                    Die ungültigen Felder wurden hervorgehoben.
                </div>
            </div>
            <!-- main form button for validation/submission -->
            <div class="button-container">
                <button class="button-primary" id="btnSubmit" type="submit">
                    Anfrage absenden
                </button>
            </div>
        </Form>
    </div>
</template>
