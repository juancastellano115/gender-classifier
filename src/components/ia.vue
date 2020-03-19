<template>
  <div class="has-text-centered box">
    <h1 class="is-size-5" style="margin-bottom: 1rem">
      Elige una foto (Selfie o similares)
    </h1>
    <div class="fileupload">
      <div class="file">
        <label class="file-label">
          <input
            class="file-input"
            type="file"
            accept="image/*"
            name="resume"
            id="file"
            @change="loadFile"
          />
          <span class="file-cta">
            <span class="file-icon">
              <i class="fas fa-upload"></i>
            </span>
            <span class="file-label">
              Choose a file…
            </span>
          </span>
        </label>
      </div>
    </div>
    <div class="container">
      <img id="output" width="200" />
    </div>
    <div class="container">
      <p v-if="show && cargando" class="subtitle">
        Cargando Modelo Sequential...
      </p>
      <div v-if="cargando == false">
        <h1 class="title is-5">
          {{ classifyContent[0].label }}
          <p>
            Precisión: {{ classifyContent[0].confidence.toPrecision(3) * 100 }}%
          </p>
        </h1>

        <p>{{ classifyContent[1].label }}</p>
        <p>
          Precisión: {{ classifyContent[1].confidence.toPrecision(2) * 100 }}%
        </p>
        <chart :chartData="chartData" />
        <footer class="footerVue">
          <p>
            Hecho con Vue 🔥, Tensorflow.JS 🤖, Keras 💻 y
            Bulma.CSS 🎨
          </p>
        </footer>
      </div>
    </div>
  </div>
</template>
<script>
import chart from "@/components/chart.vue";
export default {
  name: "ia",
  components: {
    chart
  },
  data() {
    return {
      cargando: true,
      show: false,
      classifyContent: [],
      chartData: {
        labels: [],
        datasets: [
          {
            label: "Classification",
            borderColor: "rgba(0, 0, 0, 0.5)",
            data: [],
            backgroundColor: []
          }
        ]
      }
    };
  },
  methods: {
    loadFile: function(event) {
      this.show = true;
      this.cargando = true;
      this.chartData.labels = [];
      this.chartData.datasets[0].data = [];
      this.chartData.datasets[0].backgroundColor = [];
      var image = document.getElementById("output");
      image.src = URL.createObjectURL(event.target.files[0]);
      this.classify();
    },
    classify: async function() {
      // eslint-disable-next-line no-undef
      const classifier = await ml5.imageClassifier("https://raw.githubusercontent.com/juancastellano115/gender-classifier/master/public/model/model.json");
      await classifier.classify(
        document.getElementById("output"),
        (err, results) => {
          for (const iterator of results) {
            this.chartData.labels.push(iterator.label);
            this.chartData.datasets[0].data.push(
              iterator.confidence.toPrecision(2)
            );

            if (iterator.label == "Hombre") {
              this.chartData.datasets[0].backgroundColor.push(
                "rgba(54, 162, 235, 0.2)"
              );
            } else if (iterator.label == "Mujer") {
              this.chartData.datasets[0].backgroundColor.push(
                "rgba(255, 99, 132, 0.2)"
              );
            }
          }
          this.classifyContent = results;
          this.show = false;
          this.cargando = false;
        }
      );
    }
  }
};
</script>

<style>
.fileupload {
  display: flex;
  align-content: center;
  justify-content: center;
  margin-bottom: 1em;
}
</style>
