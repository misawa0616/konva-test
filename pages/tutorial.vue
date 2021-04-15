<template>
  <div>
    <button @click="test()" value="a">a</button>
    <v-stage
      ref="stage"
      @click="clickEvent"
      @tap="clickEvent"
      :config="configKonva"
    >
      <v-layer ref="layer">
        <template v-for="(shapeConfig, index) in shapeConfigs">
          <v-circle
            v-if="shapeConfig.type == 1"
            :config="shapeConfig"
            :key="index"
          ></v-circle>
        </template>
        <v-transformer ref="transformer"></v-transformer>
      </v-layer>
    </v-stage>
  </div>
</template>
<script>
export default {
  data() {
    return {
      configKonva: {
        width: 1200,
        height: 1200,
      },
      shapeConfigs: [
        {
          x: 100,
          y: 100,
          radius: 70,
          fill: "red",
          stroke: "black",
          strokeWidth: 4,
          draggable: true,
          name: "rect",
          type: 1,
          id: 1,
        },
        {
          x: 300,
          y: 100,
          radius: 70,
          fill: "red",
          stroke: "black",
          strokeWidth: 4,
          draggable: true,
          name: "rect",
          type: 1,
          id: 2,
        },
        {
          x: 500,
          y: 100,
          radius: 70,
          fill: "red",
          stroke: "black",
          strokeWidth: 4,
          draggable: true,
          name: "rect",
          type: 1,
          id: 3,
        },
      ],
    };
  },
  methods: {
    clickEvent: function (e) {
      if (this.$refs.transformer.getNode().nodes().length >= 1) {
        this.$refs.transformer.getNode().nodes()[0].fill("red");
      }
      // if (selectionRectangle.visible()) {
      //   return;
      // }
      if (e.target === this.$refs.stage.getNode()) {
        this.$refs.transformer.getNode().nodes([]);
        this.$refs.layer.getNode().draw();
        return;
      }
      if (!e.target.hasName("rect")) {
        return;
      }
      const metaPressed = e.evt.shiftKey || e.evt.ctrlKey || e.evt.metaKey;
      const isSelected =
        this.$refs.transformer.getNode().nodes().indexOf(e.target) >= 0;
      if (!metaPressed && !isSelected) {
        this.$refs.transformer.getNode().nodes([e.target]);
      } else if (metaPressed && isSelected) {
        const nodes = this.$refs.transformer.getNode().nodes().slice();
        nodes.splice(nodes.indexOf(e.target), 1);
        this.$refs.transformer.getNode().nodes(nodes);
      } else if (metaPressed && !isSelected) {
        const nodes = this.$refs.transformer
          .getNode()
          .nodes()
          .concat([e.target]);
        this.$refs.transformer.getNode().nodes(nodes);
      }
      if (this.$refs.transformer.getNode().nodes().length >= 1) {
        this.$refs.transformer.getNode().nodes()[0].fill("blue");
      }
      this.$refs.layer.getNode().draw();
    },
    test() {
      let y = this.$refs.transformer.getNode().nodes()[0].y();
      for (let i in this.$refs.transformer.getNode().nodes()) {
        this.$refs.transformer.getNode().nodes()[i].y(y);
      }
    },
  },
};
</script>