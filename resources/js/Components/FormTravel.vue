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

import { Form, Field, ErrorMessage, configure } from "vee-validate";
import * as yup from "yup";
import { onMounted, ref } from "vue";
import flatpickr from "flatpickr";
import { German } from "flatpickr/dist/l10n/de.js";
import "flatpickr/dist/themes/light.css";

configure({
    validateOnBlur: false,
    validateOnChange: false,
});

const form = ref(null);

const formData = ref({
    travel_period: "",
    number_adults: undefined,
    child_name1: "",
    datebirth_date1: undefined,
    datebirth_month1: "",
    datebirth_year1: undefined,
});

const schema = yup.object().shape({
    travel_period: yup
        .string()
        .required("Geben Sie eine Periodzeit")
        .min(25)
        .max(25),
    number_adults: yup
        .number()
        .moreThan(0)
        .required("Geben Sie eine positive Zahl ein.")
        .typeError("Geben Sie eine positive Zahl ein."),
    child_name1: yup.string().required("Geben Sie eine Name ein."),
    datebirth_date1: yup
        .number()
        .required("Geben Sie ein Tag ein.")
        .typeError("Geben Sie ein Tag ein."),
    datebirth_month1: yup
        .number()
        .required("Geben Sie ein Monat ein.")
        .typeError("Geben Sie ein Monat ein."),
    datebirth_year1: yup
        .number()
        .required("Geben Sie ein Jahr ein.")
        .typeError("Geben Sie ein Jahr ein."),
});

onMounted(() => {
    let schowMonths = 2;
    if (window.innerWidth < 724) {
        schowMonths = 1;
    }

    flatpickr(".flatpickr", {
        mode: "range",
        minDate: "today",
        locale: German,
        showMonths: schowMonths, //need change for resize window
        wrap: true,
    });
});

const resetForm = () => {
    form.value.resetForm();
};

function onSubmitForm(values) {
    console.log(values);
    resetForm();
}
</script>

<template>
    <div class="form-traver-wrap">
        <Form ref="form" :validation-schema="schema" @submit="onSubmitForm">
            <div class="form-group">
                <div class="form-group-title sansita-swashed-regular">
                    Ihre Urlaubsdaten
                </div>
                <div class="form-group-container">
                    <div class="form-row">
                        <label class="form-row-label" for="travelperiod"
                            >Reisezeitraum *</label
                        >
                        <div class="flatpickr form-row-container">
                            <Field
                                type="text"
                                id="travelperiod"
                                name="travel_period"
                                placeholder="select period"
                                v-model="formData.travel_period"
                                required
                                data-input
                                :class="
                                    form?.errors?.travel_period
                                        ? 'form-input-error'
                                        : ''
                                "
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
                            <ErrorMessage name="travel_period" />
                        </div>
                    </div>
                    <div class="form-row">
                        <label class="form-row-label" for="numberadults"
                            >Anzahl Erwachsene *</label
                        >
                        <div class="form-row-container">
                            <Field
                                type="text"
                                id="numberadults"
                                name="number_adults"
                                placeholder=""
                                v-model.lazy="formData.number_adults"
                                required
                                min="1"
                                pattern="\d+"
                                :class="
                                    form?.errors?.number_adults
                                        ? 'form-input-error'
                                        : ''
                                "
                            />
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

                <div class="form-group-container">
                    <div class="form-row">
                        <label class="form-row-label" for="child_name1"
                            >Name des Kindes *
                        </label>
                        <div class="form-row-container">
                            <Field
                                type="text"
                                id="child_name1"
                                name="child_name1"
                                v-model.lazy="formData.child_name1"
                                :class="
                                    form?.errors?.child_name1
                                        ? 'form-input-error'
                                        : ''
                                "
                                required
                                data-input
                            />
                            <ErrorMessage name="child_name1" />
                        </div>
                    </div>
                    <div class="form-row">
                        <label class="form-row-label" :for="datebirth_date1"
                            >Geburtstag *</label
                        >
                        <div class="form-row-container">
                            <div class="form-row-date-container">
                                <Field
                                    as="select"
                                    id="datebirth_date1"
                                    name="datebirth_date1"
                                    v-model.lazy="formData.datebirth_date1"
                                    required
                                    :class="
                                        form?.errors?.datebirth_date1
                                            ? 'form-input-error'
                                            : ''
                                    "
                                >
                                    <option value="" disabled>Tag</option>
                                    <option v-for="n in 31" :key="n" :value="n">
                                        {{ n }}
                                    </option>
                                </Field>
                                <Field
                                    as="select"
                                    name="datebirth_month1"
                                    v-model.lazy="formData.datebirth_month1"
                                    required
                                    :class="
                                        form?.errors?.datebirth_month1
                                            ? 'form-input-error'
                                            : ''
                                    "
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
                                <Field
                                    as="select"
                                    name="datebirth_year1"
                                    v-model.lazy="formData.datebirth_year1"
                                    required
                                    :class="
                                        form?.errors?.datebirth_year1
                                            ? 'form-input-error'
                                            : ''
                                    "
                                >
                                    <option value="" disabled>Jahr</option>
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
                            </div>
                            <ErrorMessage name="datebirth_date1" />
                            <ErrorMessage name="datebirth_month1" />
                            <ErrorMessage name="datebirth_year1" />
                        </div>
                    </div>
                </div>

                <div class="form-row-kinder-links-container">
                    <button type="button" class="btn-increase">
                        Kind hinzufügen
                    </button>
                    <span>&#8594;</span>
                    <button type="button" class="btn-decrease">
                        Kind entfernen
                    </button>
                </div>
            </div>
            <div class="button-container">
                <button class="button-primary" id="btnSubmit" type="submit">
                    Anfrage absenden
                </button>
            </div>
        </Form>
    </div>
</template>
