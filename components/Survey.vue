<template>
  <div class="container">
    <div class="leading">
      <div class="form-container">
        <div id="sc-container">
          <div id="sc-branding" class="sc-bb">
            <a target="_blank" href="https://www.speedcheck.org/">
              <img
                src="https://cdn.speedcheck.org/branding/speedcheck-logo-18.png"
                alt="Speedcheck"
              />
            </a>
          </div>
        </div>
      </div>
      <script
        src="https://cdn.speedcheck.org/basic/scbjs.min.js"
        async
      ></script>
      <div id="survey-panel" class="panel panel-white panel-no-border hide">
        <div
          id="survey123-webform"
          class="panel panel-no-padding panel-no-border"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  mounted: function() {
    let vm = this;
    var open = window.XMLHttpRequest.prototype.open,
      send = window.XMLHttpRequest.prototype.send;

    function openReplacement(method, url, async, user, password) {
      this._url = url;
      return open.apply(this, arguments);
    }

    function sendReplacement(data) {
      /**
       * PLACE HERE YOUR CODE WHEN REQUEST IS SENT
       */
      if (this._url == "https://api.speedspot.org/basic" && arguments[0]) {
        console.log(arguments[0]);
        vm.results = JSON.parse(arguments[0]);
        vm.initializeForm(vm.results);
      }
      return send.apply(this, arguments);
    }

    window.XMLHttpRequest.prototype.open = openReplacement;
    window.XMLHttpRequest.prototype.send = sendReplacement;
  },
  data: function() {
    return {
      results: {}
    };
  },
  methods: {
    initializeForm: function(speedData) {
      const survey123WebForm = new Survey123WebForm({
        container: "survey123-webform",
        clientId: "Jy4JtM71ralXVggd",
        portalUrl: "https://www.arcgis.com",
        itemId: "f7d1b61670ec49788a8a9f246f5b2e9b",
        onFormLoaded: data => {
          //
          // ANSWER INTERNET SPEED QUESTION WITH CONNECTION SPEED INFO //
          //
          survey123WebForm.setQuestionValue({
            internet_speed: speedData.downloadValue,
            field_14: speedData.uploadValue,
            
          });
        }
      });
      // SHOW SURVEY123 FORM //
      let surveyPanel = document.getElementById("survey-panel");
      surveyPanel.classList.remove("hide");
    }
  }
};
</script>

<style lang="scss">
.leading {
  text-align: center;
  margin-top: 50px;
}
.iframecontainer {
  margin: 40px;
  background-color: rgb(26, 26, 26);
}
.form-container {
  display: flex;
  justify-content: center;
  align-items: center;
  #sc-container {
    .sc-bb {
      display: none;
    }
    .sc-button {
      cursor: pointer;
    }
    #sc-basic-link {
      display: none;
    }
    #sc-footer {
      display: none;
    }
  }
}
</style>