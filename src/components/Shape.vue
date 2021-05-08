<template>
  <div class="border" @click.self="closeShape">
    <div
      @click.self="closeShape"
      :style="style"
      class="defaultSize"
      :class="[
        color === 'green'
          ? 'colorGreen'
          : color === 'red'
          ? 'colorRed'
          : color === 'blue'
          ? 'colorBlue'
          : 'colorYellow',
        shape === 'circle'
          ? 'circle'
          : shape === 'triangular'
          ? 'triangular'
          : '',
      ]"
    ></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      clickOnGreen: false,
    };
  },

  props: ["shape", "color"],
  methods: {
    closeShape() {
      if (this.$props.color == "green") {
        this.clickOnGreen = true;
      }
      this.$emit("close", this.clickOnGreen);
      this.clickOnGreen = false;
    },
  },
  computed: {
    style() {
      return {
        "--color":
          this.color === "green"
            ? "forestgreen"
            : this.color === "red"
            ? "crimson"
            : this.color === "blue"
            ? "royalblue"
            : "gold",
      };
    },
  },
};
</script>

<style scoped>
.border {
  border: 1px solid black;
  width: 250px;
  height: 210px;
  margin: auto auto;
}
.defaultSize {
  width: 200px;
  height: 120px;
  padding: 20px;
  margin: 20px auto;
  border-radius: 10px;
}
.colorGreen {
  background: forestgreen;
}
.colorRed {
  background: crimson;
}
.colorBlue {
  background: royalblue;
}
.colorYellow {
  background: gold;
}
.circle {
  padding: 0;
  margin: 5px auto;
  height: 200px;
  border-radius: 50%;
}
.triangular {
  margin: 5px auto;
  width: 0;
  height: 0px;
  padding: 0px;
  background: none;
  border-left: 100px solid transparent;
  border-right: 100px solid transparent;
  border-bottom: 200px solid var(--color);
}
</style>