<template>
  <v-container fluid class="pa-10 block-two base-black">
    <v-row justify="center" class="custom-width">
      <v-col cols="10" v-for="it in items" :key="it.id">
        <div class="my-text parallax-content text-left">
          <div class="text-h4 font-weight-medium mb-4 header-font">
            {{ it.sLabel }}
            <span class="header-font" :class="`text-${it.id}`">{{
              it.mLabel
            }}</span>
          </div>
          <p
            v-for="sIt in it.subItems"
            :key="sIt.id"
            class="font-weight-regular mb-1 custom-p"
          >
            {{ sIt.label }}
          </p>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { ref, onMounted } from "vue";
export default {
  name: "BlockTwo",
  setup() {
    const items = ref([
      {
        id: 1,
        sLabel: "We believe in the power of spaces that",
        mLabel: "Indulge.",
        subItems: [
          {
            id: 11,
            label:
              "Indulgence is us weaving your stories into your homes Introducing narratives.",
          },
        ],
      },
      {
        id: 2,
        sLabel:
          "Presenting a magical twist to interiors, we're Spellbound - a studio crafting beyond",
        mLabel: "designs.",
        subItems: [
          {
            id: 21,
            label:
              "We've been weaving narratives into spaces since over a decade & more Spatially & Sensorially, All that we do",
          },
        ],
      },
    ]);

    onMounted(() => {
      class TextScramble {
        constructor(el) {
          this.el = el;
          this.chars = "___________________________";
          this.update = this.update.bind(this);
        }
        setText(newText) {
          const oldText = this.el.innerText;
          const length = Math.max(oldText.length, newText.length);
          const promise = new Promise((resolve) => (this.resolve = resolve));
          this.queue = [];
          for (let i = 0; i < length; i++) {
            const from = oldText[i] || "";
            const to = newText[i] || "";
            const start = Math.floor(Math.random() * 40);
            const end = start + Math.floor(Math.random() * 40);
            this.queue.push({ from, to, start, end });
          }
          cancelAnimationFrame(this.frameRequest);
          this.frame = 0;
          this.update();
          return promise;
        }
        update() {
          let output = "";
          let complete = 0;
          for (let i = 0, n = this.queue.length; i < n; i++) {
            let { from, to, start, end, char } = this.queue[i];
            if (this.frame >= end) {
              complete++;
              output += to;
            } else if (this.frame >= start) {
              if (!char || Math.random() < 0.28) {
                char = this.randomChar();
                this.queue[i].char = char;
              }
              output += `<span class="dud">${char}</span>`;
            } else {
              output += from;
            }
          }
          this.el.innerHTML = output;
          if (complete === this.queue.length) {
            this.resolve();
          } else {
            this.frameRequest = requestAnimationFrame(this.update);
            this.frame++;
          }
        }
        randomChar() {
          return this.chars[Math.floor(Math.random() * this.chars.length)];
        }
      }

      const phrases1 = [
        "Indulge.",
        "Enchant.",
        "Elevate.",
        "Captivate.",
        "Enrich.",
      ];

      const el1 = document.querySelector(".text-1");
      const fx1 = new TextScramble(el1);

      let counter1 = 0;
      const next1 = () => {
        fx1.setText(phrases1[counter1]).then(() => {
          setTimeout(next1, 1500);
        });
        counter1 = (counter1 + 1) % phrases1.length;
      };

      next1();

      const phrases2 = ["designs.", "concepts.", "visions.", "imaginations."];

      const el2 = document.querySelector(".text-2");
      const fx2 = new TextScramble(el2);

      let counter2 = 0;
      const next2 = () => {
        fx2.setText(phrases2[counter2]).then(() => {
          setTimeout(next2, 1500);
        });
        counter2 = (counter2 + 1) % phrases2.length;
      };

      next2();
    });

    return {
      items,
    };
  },
};
</script>

<style lang="scss" scoped>
.my-text {
  font-size: 18px;
  color: #333;
  margin-bottom: 20px;
}
.parallax-content {
  color: white !important;
}
.custom-p {
  font-size: 18px;
}
.block-two {
  padding-top: 85px !important;
  padding-bottom: 85px !important;
}
.custom-width {
  max-height: 1100px !important;
}
</style>
