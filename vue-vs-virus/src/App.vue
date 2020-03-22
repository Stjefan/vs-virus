<template>
  <v-app style="background-color: #F5F5F5;">
    <v-app-bar app color="#9dd4b1" dark>
      <div class="d-flex align-center">
        <v-img
          v-if=true
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="..\src\assets\Thumbnail_v3.png"
          transition="scale-transition"
          width="100"
        />

        <p style="color: white; font-size: 30px;">bienenstock </p>
      </div>

      <v-spacer></v-spacer>
      <div class="d-flex align-center">
        <v-img
          v-if=true
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="..\src\assets\Bee_Hex_Right_1.png"
          transition="scale-transition"
          width="100"
        />
      </div>
      <div class="d-flex align-center">
        <v-img
          v-if=true
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="..\src\assets\Bee_Hex_Left.png"
          transition="scale-transition"
          width="100"
        />
      </div>
    </v-app-bar>

    <div
      style="padding-top: 100px; padding-left: 20px; padding-right: 30px; padding-bottom: 100px; background-color: white"
    >
      <v-content style="background-color: #9dd4b1; padding: 20px;">
        <h1>Eine Anfrage erstellen</h1>
        <div class="padding">
          <h3>Meine persönlichen Daten</h3>
          <v-text-field class="vtextfield" v-model="vorname" label="Vorname" />
          <v-text-field class="vtextfield" v-model="nachname" label="Nachname" />
          <v-text-field class="vtextfield" v-model="adresse" label="Addresse" />
          <v-text-field class="vtextfield" v-model="phone" label="Telefon" />
          <v-btn @click="abc = !abc">{{zustandDatenschutzerklaerung}} Datenschutzerklärung</v-btn>
          <div v-if="abc"> {{mlString}} </div>
        </div>
        <div class="padding">
          <h3>Mein Anliegen</h3>

          <v-select
            class="vselect"
            label="Kategorie wählen"
            v-model="req_category"
            :items="options"
            item-value="kategorie"
            item-text="kategorie"
          >
            <option disabled value="">Bitte Kategorie auswählen</option>
            <option v-for="op in options" :key="op.kategorie">
              {{ op.kategorie }}
            </option>
          </v-select>
          <v-select
            class="vselect"
            label="Unterkategorie wählen"
            v-if="req_category_options"
            v-model="req_category_specified"
            :items="req_category_options"
            item-value="kategorie"
            item-text="kategorie"
          >
          </v-select>
          <v-text-field label="Zusätzliche Anmerkungen" v-model="req_category_text" class="vtextfield large" />
        </div>
         <div style="width: 100%">
          <v-btn :loading="loading" @click="loader = 'loading'">Anfrage abschicken</v-btn>
         <v-container fluid style="width: 70%; float: right">
           <v-row>
             <v-col v-for="n in 5"
              :key="n">
         <v-img
          v-if=true
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="..\src\assets\Bee_Hex_Left.png"
          transition="scale-transition"
          :width="getRandomArbitrary()"
        />
        </v-col>
           </v-row>
         </v-container>
        </div>
      </v-content>
    </div>
  </v-app>
</template>

<script>
//import Input200320 from "./components/Input200320";
import axios from "axios";

export default {
  name: "App",

  components: {
    //Input200320
  },

  data() {
    return {
      abc: false,
      zeigeDatenschutzerklaerung: true,
      loader: null,
      loading: null,
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
      req_user_id: null,
      mlString: "Datenschutzerklärung\nDiese Datenschutzerklärung gilt für die mobile App „Bienenstock“, den Telefonchatroboter „Willi“ und die Webseiten http://18.194.159.113:8080/. Diese Datenschutzerklärung informiert Sie darüber, wie wir personenbezogenen Daten im Zusammenhang mit unseren Diensten erheben und verarbeiten.\n\n§ 1. Verantwortliche, Art. 24 I DSGVO\nHinsichtlich der Dienste ist die ### Verantwortliche im Sinne der Allgemeinen Datenschutz- Grundverordnung (“DSGVO”).\n§ 2. Datenerhebung und Verarbeitung, Art. 5ff. DSGVO\na. App\naa. Die Nutzung unserer App ist ohne die Angabe von personenbezogenen Daten, iSd DSGVO, wie z.B. Namen, E-Mail-Adresse, Postanschrift und Telefonnummer möglich.\nbb. Bei der Nutzung der App wird für Sie auf Basis Ihres mobilen Geräts eine zufällige Benutzernummer (Hash-Wert) generiert, die es uns ermöglicht, Ihre Bienenstock-Aktivitäten (Laufende Aufträge) ihrem Gerät zuzuordnen. Wir können von dieser Nummer oder von dem Account nicht auf Ihren Namen oder sonstige personenbezogenen Daten schließen.\ncc. Darüber hinaus sind die vollen Funktionalitäten der App nur zugänglich, wenn Sie der App den Zugriff auf den Standort Ihres mobilen Endgeräts gewähren. Zu diesem Zweck werden Sie bei der ersten Nutzung der App von Ihrem Gerät gefragt, ob Standortdaten verwendet werden dürfen. Wenn Sie dies nicht zulassen oder wenn Sie es zulassen, diese Funktionalität aber später in den Einstellungen Ihres Mobilgeräts deaktivieren.\n\nb. Website\naa. Wenn Sie die Webseite besuchen, überträgt Ihr Browser automatisch bestimmte Daten, um die Webseite zu öffnen, insbesondere: ###\nbb. Wenn Sie uns Informationen auf der Website, über das auf der Website enthaltenes Auftragsformular oder auf andere Weise zur Verfügung stellen, dann werden die angegebene Adresse, Telefonnummer und der Name für die Auftragserstellung gespeichert. Dies ist erforderlich, damit User die Aufträge annehmen können.\nc. Telephonchatbot „Willi“\naa. Für die Nutzung von Willi ist die Erhebung und Verarbeitung folgender personenbezogener Daten notwendig: Name; Vorname; Adresse und Telefonnummer. Vor dieser Erhebung wird durch unseren automatisierten Dialog die Einwilligung iSd Art. 7 I DSGVO.\n§ 3. Rechtsgrundlage für die Verarbeitung von personenbezogenen Daten, Art. 6 DSGVO\n•	Rechtsgrundlage für § 2.a.bb (in dem Maße, in dem diese Daten als personenbezogene Daten gelten) ist Art. 6 I 1 lit. f DSGVO. Das berechtigte Interessen ergibt sich daraus, dass zur Nutzung, dass die Nutzbarkeit der App sonst nicht gewährleistet werden kann.\n•	Rechtsgrundlage für § 2.a.cc Art. 6 I 1 lit. b und lit f. Das berechtigte Interesse besteht darin, dass wir die App-Services mit sämtlichen Funktionalitäten nur anbieten können, wenn entsprechende Geo-Positionsdaten verarbeitet werden. Wir werden Ihre Geo-Positionsdaten nicht verarbeiten, wenn Sie Ihrem Gerät nicht gestatten.\n•	Rechtsgrundlage für § 2.b.aa (in dem Maße, in dem diese Daten als personenbezogene Daten gelten) ist Art. 6 I 1 lit. b DSGVO.\n•	Rechtsgrundlage für § 2.b.bb ist DSGVO Art, 6 I 1 lit f. DSGVO. Das berechtigte Interesse besteht darin, dass wir die entsprechenden Daten verarbeiten müssen, um Ihre Aufträge hochzuladen;\n•	Rechtsgrundlage für § 2.c ist Art. 6 I 1 lit. a DSGVO. Die Einwilligung iSd Art. 7 DSGVO wird im Rahmen des Gesprächs abgegeben.\n•	\n§ 4. Analyse- und Werbesoftware\nDie Daten werden an keine Dritten weitergegeben. Bienenstock nutzt keine Werbe- oder Analyse-Diente.\n§ 5. Auftragsverarbeitung durch Dritte, Art. 28 DSGVO\nDie Daten werden an keine Drittanbieter zur Auftragsverarbeitung weitergegeben.\n§ 6. Dauer der Speicherung Personenbezogener Daten\n###\n§ 7. Ihre Rechte, Art. 12 ff. DSGVO\nSie haben das Recht, jederzeit Informationen über Ihre bei uns gespeicherten personenbezogenen Daten anzufordern. Falls die gesetzlichen Bestimmungen erfüllt sind, haben Sie uns gegenüber zudem das Recht auf Auskunft und Berichtigung, Löschung oder Sperrung der Verarbeitung Ihrer personenbezogene Daten oder auf Einspruch gegen die Verarbeitung Ihrer personenbezogenen Daten sowie das Recht, Ihre personenbezogenen Daten bei uns in einem strukturierten, etablierten und maschinenlesbaren Format anzufordern (Sie können diese Daten an Drittparteien übermitteln oder übermitteln lassen; Datenübertragbarkeit).\nWenn Sie Ihre Zustimmung zur Verwendung Ihrer personenbezogenen Daten erteilt haben, können Sie diese Zustimmung jederzeit (für die Zukunft) widerrufen.\nFalls Sie der Ansicht sind, dass die Verarbeitung Ihrer personenbezogenen Daten durch uns gegen geltende Datenschutzvorschriften verstößt, können Sie eine Beschwerde bei der zuständigen Aufsichtsbehörde für Datenschutz einreichen.\n§ 8. Kontakt; Datenschutzbeauftragter\n###\n§ 9. Sicherung der Personenbezogenen Daten\n###\n\nStand: März 2020",

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
    },
    getRandomArbitrary: function() {
      return Math.floor(Math.random() * (120 - 80) + 80);
    }
  },
   watch: {
      loader () {
        const l = this.loader
        this[l] = !this[l]
        this.doFancyStuff()
        this[l] = false
        this.loader = null
      },
    },
  computed: {
    zustandDatenschutzerklaerung() {
      if (this.abc) {
        return "Verberge"
      } else {
        return "Zeige"
      }
    },
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
  color: white;
}

.vselect {
  width: 50%;
}
.vinput {
  width: 50%;
}
.vtextfield {
  width: 50%;
}
v-text-field {
  width: 50%;
}
.padding {
  padding: 20px;
}
</style>
