<template>
  <div class="c-table">
    <div class="c-table__deck-of-card"></div>
    <div class="c-table__flop">
      <div
        class="c-table__card"
        :class="{
          'c-table__card--flipped': card.flipped,
          'c-table__card--raised': card.raised,
        }"
        v-for="card in flop"
        :key="card.id"
      >
        <div class="c-table__card-inner">
          <div class="c-table__card-front">
            <div class="c-table__card-value">{{ card.value }}</div>
            <div class="c-table__card-suit">{{ card.suit }}</div>
          </div>
          <div class="c-table__card-back"></div>
        </div>
      </div>
    </div>
    <div class="c-table__actions">
      <button @click="flipAndRaiseCard">Flip and rise middle card</button>
    </div>
    <div class="c-table__bets">
      <button class="c-table__bet c-table__bet--low">
        <span>Low or Same</span>
      </button>
      <button class="c-table__bet c-table__bet--high">
        <span>High or Same</span>
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

const flop = ref<
  {
    id: number;
    suit: string;
    value: string;
    flipped: boolean;
    raised: boolean;
  }[]
>([
  { id: 1, suit: "hearts", value: "A", flipped: false, raised: false },
  { id: 2, suit: "hearts", value: "K", flipped: false, raised: false },
  { id: 3, suit: "hearts", value: "Q", flipped: false, raised: false },
]);

const flipMiddleCard = () => {
  flop.value[1].flipped = true;
};

const raiseMiddleCard = () => {
  flop.value[1].raised = true;
};

const flipAndRaiseCard = () => {
  flipMiddleCard();
  setTimeout(() => {
    raiseMiddleCard();
  }, 700);
};
</script>

<style lang="scss">
@mixin card-back {
  border-radius: var(--card-border-radius);

  &::before {
    content: "";
    position: absolute;
    z-index: -1;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    border-radius: var(--card-border-radius);
    background-color: var(--card-border-color);
  }

  &::after {
    content: "";
    position: absolute;
    top: var(--card-border-size);
    left: var(--card-border-size);
    right: var(--card-border-size);
    bottom: var(--card-border-size);
    border-radius: var(--card-border-radius);
    background-color: var(--card-back-color);
  }
}

.c-table {
  --card-gap: 10px;
  --card-border-radius: 10px;
  --card-back-color: #000;
  --card-border-color: #333;
  --card-border-size: 8px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-end;
  position: relative;

  @media (min-width: 992px) {
    --card-width: 185px;
    --card-height: 277px;
    --flop-gap: 50px;


    width: calc(5 * var(--card-width) + 4 * var(--card-gap));
    height: calc(2 * var(--card-height) + var(--flop-gap));
  }

  @media (max-width: 991px) {
    --card-width: 118px;
    --card-height: 178px;
    --flop-gap: 40px;

    width: calc(3 * var(--card-width) + 2 * var(--card-gap));
    height: calc(2 * var(--card-height) + var(--flop-gap));
  }
}

.c-table__flop {
  margin-top: auto;
  display: flex;
  gap: var(--card-gap);
  position: relative;
  z-index: 2;
}

.c-table__card {
  width: var(--card-width);
  height: var(--card-height);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
  perspective: 1000px;
  border-radius: var(--card-border-radius);
}

.c-table__card-inner {
  width: 100%;
  height: 100%;
  transition: transform 0.7s;
  transform-style: preserve-3d;
  position: relative;
  perspective: inherit;
  border-radius: var(--card-border-radius);
}

.c-table__card-back,
.c-table__card-front {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  border-radius: var(--card-border-radius);
}

.c-table__card-front {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: absolute;
  background-color: cadetblue;
  transform: rotateY(-180deg);
}

.c-table__card-back {
  @include card-back;
}

.c-table__card--flipped .c-table__card-inner {
  transform: rotateY(180deg);
}

.c-table__card--raised .c-table__card-inner {
  transform: rotateY(180deg) translateY(calc(-1 * var(--flop-gap) - 100%));
}

.c-table__card-value {
  font-size: 2rem;
  margin-bottom: 2rem;
  position: relative;
}

.c-table__deck-of-card {
  @include card-back;
  width: var(--card-width);
  height: var(--card-height);
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  margin-left: auto;
  margin-right: auto;
}

.c-table__actions {
  position: absolute;
  z-index: 1;

  @media (min-width: 992px) {
    bottom: 0;
  right: 0;
  width: var(--card-width);
  }

  @media (max-width: 991px) {
    top: 0;
    left: 0;
    width: 100%;
  }
}

.c-table__bets {
  position: absolute;
  left: 0;
  right: 0;
  height: var(--card-height);
  display: flex;
  justify-content: space-between;
  align-items: center;

  @media (min-width: 992px) {
    top: auto;
    bottom: 0;
  }

  @media (max-width: 991px) {
    top: 0;
    bottom: auto;
  }
}

.c-table__bet {
  background: transparent;
  border: none;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.c-table__bet::before {
  content: "";
  width: var(--size);
  height: var(--size);
  background-color: var(--color, transparent);
  cursor: pointer;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2rem;

  @media (min-width: 992px) {
    --size: 64px;
    margin-bottom: 10px;
  }

  @media (max-width: 991px) {
    --size: 48px;
    margin-bottom: 6px;
  }
}

.c-table__bet--low::before {
  content: "↓";
  background-color: #f00;
}

.c-table__bet--high::before {
  content: "↑";
  background-color: #0f0;
}
</style>
