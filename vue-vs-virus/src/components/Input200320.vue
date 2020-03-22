<template>
  <div class="input-component"
    style="width: 100%; margin-left: auto;
    margin-right: auto; height: 100%;"
  >
    <h1>Biene an Bienenschwarm</h1>
    <div class="topnav">
      <a href="#">Link</a>
      <a href="#">Impressum</a>
      <a href="#" style="float:right">Link</a>
    </div>

    <div style="background-color: #FFDC49;">
      <div style="border: 1px solid black; padding-left: 10px; padding-right: 10px; padding-bottom: 10px; padding-top: 40px;">
        <p>
          Vorname:
        </p>
        <input v-model="vorname" />
        <p>
          Nachname:
        </p>
        <input v-model="nachname" />
        <p>
          Addresse:
        </p>
        <input v-model="adresse" />
        <p>
          Telefon:
        </p>
        <input v-model="phone" />
        <button @click="console.log($event)">
          Datenschutz anzeigen
        </button>
        <label class="container">Ich bin mit der Datenverarbeitung meiner Daten einverstanden.
  <input type="checkbox">
  <span class="checkmark"></span>
</label>
      </div>
      <div style="border: 1px solid black; padding: 10px">
        <p>
          Anliegen:
        </p>
        <select v-model="req_category">
          <option disabled value="">Bitte Kategorie auswählen</option>
          <option v-for="op in options" :key="op.kategorie">
            {{ op.kategorie }}
          </option>
        </select>
        <select v-if="req_category_options" v-model="req_category_specified">
          <option disabled value="">Bitte Unterkategorie auswählen</option>
          <option v-for="op in req_category_options" :key="op.kategorie">
            {{ op.kategorie }}
          </option>
        </select>
        <p>
          Zusatztext:
        </p>
        <input v-model="req_category_text" id="largeInput" />
      </div>
      <div style="border: 1px solid black; padding: 10px">
        <button @click="doFancyStuff($event)">
          Anfrage veröffentlichen
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Input200320",
  props: {
    msg: String
  },
  data() {
    return {
      options: [
        { kategorie: "Einkaufen/Besorgungen" },
        { kategorie: "Soziales" },
        { kategorie: "Informationen" },
        { kategorie: "Allgemeines" }
      ],
      vorname: null,
      nachname: null,
      adresse: null,
      phone: null,
      req_category: null,
      req_category_specified: null,
      req_category_text: null,
      req_user_id: null
    };
  },
  methods: {
    doFancyStuff: function(arg) {
      // http://18.194.159.113:5001/api/new_user/?phone=+4917622818&vorname=Jochen&nachname=Luithardt&adresse=Linden%C3%A4ckerstra%C3%9Fe&req_category=Einkauf&req_category_text=Ich%20brauch%20hilfe%20bei%20meinem%20Einkauf
      // http://18.194.159.113:5001/api/new_user/?req_user_id=a67e0b1d-4d34-d9ae-ed30-77c60e82c623&phone=+4917622818&vorname=Jochen&nachname=Luithardt&adresse=Lindenäckerstraße&req_category=Einkauf&req_category_text=Ich%20brauch%20hilfe%20bei%20meinem%20Einkauf
      console.log("API-Request by ", arg);
      axios
        .get(`http://18.194.159.113:5001/api/new_user_request/?`, {
          params: {
            req_user_id: "".concat(
              "a67e0b1d-4d34-d9ae-ed30-",
              Math.floor(Math.random() * 100000000)
            ),
            phone: this.phone,
            vorname: this.vorname,
            nachname: this.nachname,
            adresse: this.adresse,
            req_category: this.req_category,
            req_category_text: this.req_category_text
          }
        })
        .then(response => {
          // JSON responses are automatically parsed.
          console.log(response);
        })
        .catch(e => {
          console.log(e);
        });
    }
  },
  computed: {
    req_category_options() {
      switch (this.req_category) {
        case "Einkaufen/Besorgungen":
          return [
            { kategorie: "Lebensmittel" },
            { kategorie: "Drogerie" },
            { kategorie: "Medikamente" },
            { kategorie: "Allgemeines" }
          ];
        case "Soziales":
          return [
            { kategorie: "Corona" },
            { kategorie: "Familie" },
            { kategorie: "Allgemeines" }
          ];
        case "Kurze Frage":
          return null;
        default:
          return null;
      }
    }
  }
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css?family=Tangerine");
@import url("https://fonts.googleapis.com/css?family=Didact+Gothic");
* {
  /*font-family: 'Tangerine', serif;*/
  font-family: "Century Gothic", "Futura", san-serif;
  font-size: large;
  color: #57b7ff;
}
select {
  width: 100%;
  padding-bottom: 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  margin-top: 0px;
  margin-bottom: 0px;
  resize: vertical;
}
input {
  width: 100%;
  padding-bottom: 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  margin-top: 0px;
  margin-bottom: 6px;
  resize: vertical;
}
button {
  min-width: 250px;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  margin-top: 6px;
  margin-bottom: 16px;
  resize: vertical;
}
/* Style the topnav links */
.topnav a {
  float: left;
  display: block;
  color: #f2f2f2;
  text-align: center;
  padding: 14px 16px;
  text-decoration: none;
}

/* Change color on hover */
.topnav a:hover {
  background-color: #ddd;
  color: black;
}
.container {
  display: block;
  position: relative;
  padding-left: 35px;
  margin-bottom: 12px;
  cursor: pointer;
  font-size: 22px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

/* Hide the browser's default checkbox */
.container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

/* Create a custom checkbox */
.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  background-color: #eee;
}

/* On mouse-over, add a grey background color */
.container:hover input ~ .checkmark {
  background-color: #ccc;
}

/* When the checkbox is checked, add a blue background */
.container input:checked ~ .checkmark {
  background-color: #2196F3;
}

/* Create the checkmark/indicator (hidden when not checked) */
.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

/* Show the checkmark when checked */
.container input:checked ~ .checkmark:after {
  display: block;
}

/* Style the checkmark/indicator */
.container .checkmark:after {
  left: 9px;
  top: 5px;
  width: 5px;
  height: 10px;
  border: solid white;
  border-width: 0 3px 3px 0;
  -webkit-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  transform: rotate(45deg);
}
</style>
