<script setup>
import { ref, reactive, onMounted, watch, computed } from "vue";

const movies = ref([
  {
    title: "The Matrix",
    year: "1999-03-31",
    img: "https://people.com/thmb/kqiBr5FPcG_UnsgN6b08fvXY7H4=/750x0/filters:no_upscale():max_bytes(150000):strip_icc():focal(749x0:751x2):format(webp)/keanu-reeves-b000b57ec9774787bdfe01d50658c867.jpg",
    order: 4,
    isCorrect: null,
  },
  {
    title: "Avatar",
    year: "2009-12-18",
    img: "https://media.wired.com/photos/639b95f7b0b422ebbe76e40b/4:3/w_2131,h_1598,c_limit/Cul-avatar.jpg",
    order: 7,
    isCorrect: null,
  },
  {
    title: "Eternal Sunshine of the Spotless Mind",
    year: "2004-04-16",
    img: "https://media.timeout.com/images/101620085/750/422/image.jpg",
    order: 5,
    isCorrect: null,
  },
  {
    title: "Interstellar",
    year: "2014-11-07",
    img: "https://media.vogue.com.tw/photos/62e8a2a3703ce598b61f9e5e/16:9/w_1280,c_limit/interstellar-01.jpg",
    order: 8,
    isCorrect: null,
  },
  {
    title: "The Terminator",
    year: "1985-03-29",
    img: "https://resizing.flixster.com/DisYrRdGVNVDPr1BrFlvVxLN81o=/300x300/v2/https://flxt.tmsimg.com/assets/p7764_k_h10_aa.jpg",
    order: 3,
    isCorrect: null,
  },
  {
    title: "I, Robot",
    year: "2004-07-16",
    img: "https://i.kym-cdn.com/entries/icons/mobile/000/025/172/irobot.jpg",
    order: 6,
    isCorrect: null,
  },
  {
    title: "Blade Runner",
    year: "1983-10-22",
    img: "https://d1nslcd7m2225b.cloudfront.net/Pictures/1024x536/8/0/8/1270808_bladerunner20497_903790.jpg",
    order: 2,
    isCorrect: null,
  },
]);

const cardContainer = ref(null);
const card = ref(null);
const timelineCards = ref([]);
const cardX = ref(0);
const cardY = ref(0);
const cardWidth = ref(0);
const cardHeight = ref(0);
const cardPosition = reactive({
  leftTop: { x: undefined, y: undefined },
  leftBottom: { x: undefined, y: undefined },
  rightTop: { x: undefined, y: undefined },
  rightBottom: { x: undefined, y: undefined },
});
const draggingStyle = ref({});
const mousePos = reactive({ x: undefined, y: undefined });
const isMoveInTimeline = ref(false);
const isOverOutline = ref(false);
const overOutlineCount = ref(0);

const timelineContainer = ref(null);
const timeline = ref(null);
const timelineStyles = ref(["", "", "", "", "", "", "", ""]);

const gameStep = ref(0);
const gameScore = ref(0);

const currentCard = ref({});
const cardLists = ref([
  {
    title: "2001: A Space Odyssey",
    year: "1968-04-02",
    img: "https://classic.imgix.net/movies/thumbnails/2001-space-odyssey-backdrop-2.jpg?auto=compress,format&v=20221026",
    order: 1,
    isCorrect: null,
  },
]);

const groupAnimated = ref(false);

const gameInit = () => {
  currentCard.value = movies.value[gameStep.value];
};

const dragstart_handler = (event) => {
  currentDraggingStyle.value = {
    position: "absolute",
    zIndex: 1000,
  };

  document.body.append(card.value);

  const moveAt = (pageX, pageY) => {
    currentDraggingStyle.value = {
      position: "absolute",
      zIndex: 1000,
      left: pageX - card.value.offsetWidth / 2 + "px",
      top: pageY - card.value.offsetHeight / 2 + "px",
    };

    cardWidth.value = card.value.offsetWidth;
    cardHeight.value = card.value.offsetHeight;
    cardX.value = pageX - card.value.offsetWidth / 2 + "px";
    cardY.value = pageY - card.value.offsetHeight / 2 + "px";
    cardPosition.leftTop = {
      x: pageX - card.value.offsetWidth / 2,
      y: pageY - card.value.offsetHeight / 2,
    };
    cardPosition.leftBottom = {
      x: pageX - card.value.offsetWidth / 2,
      y: pageY + card.value.offsetHeight / 2,
    };
    cardPosition.rightTop = {
      x: pageX + card.value.offsetWidth / 2,
      y: pageY - card.value.offsetHeight / 2,
    };
    cardPosition.rightBottom = {
      x: pageX + card.value.offsetWidth / 2,
      y: pageY + card.value.offsetHeight / 2,
    };
  };

  moveAt(event.pageX, event.pageY);

  const dragmove_handler = (ev) => {
    moveAt(ev.pageX, ev.pageY);
    handleTimelineObserver();
  };
  document.addEventListener("mousemove", dragmove_handler);
  card.value.addEventListener("mouseup", () => {
    document.removeEventListener("mousemove", dragmove_handler);

    groupAnimated.value = false;
    if (isMoveInTimeline.value) {
      cardLists.value.splice(overOutlineCount.value, 0, currentCard.value);
      cardLists.value[overOutlineCount.value].isCorrect = judgeOrder(
        overOutlineCount.value
      );

      setTimeout(() => {
        groupAnimated.value = true;
        cardLists.value.sort(function (a, b) {
          return a.order - b.order;
        });
      }, 1000);
      updateStep();
    }
    cardContainer.value.append(card.value);
    currentDraggingStyle.value = {
      position: "static",
      zIndex: 0,
      left: "auto",
      top: "auto",
      transition: "all 2s ease-in-out",
    };
    isMoveInTimeline.value = false;
    isOverOutline.value = false;
    overOutlineCount.value = 0;
    updateTimelineStyles();
    card.value.removeEventListener("mouseup", () => {});
  });
};

const updateStep = () => {
  if (gameStep.value < movies.value.length) {
    gameStep.value = gameStep.value + 1;
  }
  currentCard.value = movies.value[gameStep.value];
};

const currentDraggingStyle = computed({
  get: () => {
    return draggingStyle.value;
  },
  set: (val) => {
    draggingStyle.value = val;
  },
});

const handleTimelineObserver = (entries) => {
  const rect = timeline.value.getBoundingClientRect();
  const rectTop = rect.top + window.scrollY;

  const cardCenterlines = timelineCards.value.map((timelineCard) => {
    let cardInfo = timelineCard.getBoundingClientRect();
    return Math.ceil(cardInfo.top + window.scrollY + cardInfo.height / 2);
  });

  const firstCard = timelineCards.value[0].getBoundingClientRect();
  const firstCardTop = firstCard.top + window.scrollY;
  const firstCardHeight = firstCard.height;

  if (cardPosition.rightBottom.y > rectTop) {
    isMoveInTimeline.value = true;
  } else {
    isMoveInTimeline.value = false;
  }
  overOutlineCount.value = cardCenterlines.reduce(
    (totalCount, cardCenterline) => {
      if (cardPosition.rightBottom.y > cardCenterline) {
        return (totalCount = totalCount + 1);
      } else {
        return totalCount;
      }
    },
    0
  );
  isOverOutline.value = overOutlineCount.value > 0;
  updateTimelineStyles();
};

const outlineStyles = computed(() => {
  const oneCard = timelineCards.value.length
    ? timelineCards.value[0].getBoundingClientRect()
    : null;
  const oneCardHeight = oneCard?.height ? oneCard.height : 0;
  return isOverOutline.value
    ? `transform: translate(0, ${
        (oneCardHeight + 8) * overOutlineCount.value
      }px);`
    : "";
});

const updateTimelineStyles = () => {
  cardLists.value.forEach((cardList, index) => {
    const outlineCard = card.value ? card.value.getBoundingClientRect() : null;
    const outlineCardHeight = outlineCard?.height ? outlineCard.height : 0;
    if (index < overOutlineCount.value) {
      timelineStyles.value[index] = `transform: translate(0, -${
        outlineCardHeight + 8
      }px);`;
    } else {
      timelineStyles.value[index] = ``;
    }
  });
};

const getTimelineStyle = (index) => {
  return timelineStyles.value[index];
};

const judgeOrder = (newInsertCardIndex) => {
  let isCorrect = false;

  if (newInsertCardIndex === 0) {
    isCorrect =
      cardLists.value[newInsertCardIndex].order <
      cardLists.value[newInsertCardIndex + 1].order;
  } else if (newInsertCardIndex === cardLists.value.length - 1) {
    isCorrect =
      cardLists.value[newInsertCardIndex].order >
      cardLists.value[newInsertCardIndex - 1].order;
  } else {
    isCorrect =
      cardLists.value[newInsertCardIndex].order <
        cardLists.value[newInsertCardIndex + 1].order &&
      cardLists.value[newInsertCardIndex].order >
        cardLists.value[newInsertCardIndex - 1].order;
  }
  return isCorrect;
};

onMounted(() => {
  card.value.addEventListener("mousedown", dragstart_handler);
  card.value.addEventListener("dragstart", () => {
    return false;
  });

  window.addEventListener("mousemove", (event) => {
    mousePos.x = event.clientX;
    mousePos.y = event.clientY;
  });
});

defineProps({
  msg: String,
});

gameInit();
</script>

<template>
  <div class="fixed right-0 top-0">
    滑鼠位置 X: {{ mousePos.x }}, Y:{{ mousePos.y }}
  </div>
  <div class="fixed right-0 top-10">
    卡片四個角位置<br />
    leftTop X: {{ cardPosition.leftTop.x }}, Y: {{ cardPosition.leftTop.y
    }}<br />
    rightTop X: {{ cardPosition.rightTop.x }}, Y: {{ cardPosition.rightTop.y
    }}<br />
    rightBottom X: {{ cardPosition.rightBottom.x }}, Y:
    {{ cardPosition.rightBottom.y }}<br />
    leftBottom X: {{ cardPosition.leftBottom.x }}, Y:
    {{ cardPosition.leftBottom.y }}<br />
  </div>
  <div class="scoreboard-container flex">
    <div>{{ gameStep }} of {{ movies.length }}</div>
    <div>
      <ul class="flex m-2">
        <li
          class="w-10 h-2 rounded-md border mr-2 flex items-center justify-center"
          v-for="step in movies.length"
        >
          {{ step }}
        </li>
      </ul>
    </div>
    <div>{{ gameScore }} points</div>
  </div>
  <div
    ref="cardContainer"
    class="current-card flex justify-center mt-10 w-80 h-32 absolute left-1/2 transform -translate-x-1/2 z-10"
  >
    <div
      ref="card"
      class="card w-80 h-32 border rounded-md p-4 flex cursor-grab relative"
      :style="currentDraggingStyle"
    >
      <div class="left w-32 border h-full">
        <img :src="currentCard.img" class="h-full object-cover" alt="" />
      </div>
      <div class="w-60 px-4">{{ currentCard.title }}</div>
      <div class="absolute right-0 bottom-10">
        座標：{{ cardX }}/{{ cardY }}
      </div>
      <div class="absolute right-0 bottom-2">
        卡片長寬：{{ cardWidth }}/{{ cardHeight }}
      </div>
    </div>
  </div>
  <div
    ref="timelineContainer"
    class="timeline flex flex-col items-center my-20 relative border border-red-300 py-10 transition-all"
    :class="isMoveInTimeline ? 'mt-0 h-[800px]' : 'mt-[200px] h-[600px]'"
  >
    <div class="absolute top-2">Before</div>
    <div ref="timeline" class="timeline absolute bottom-0 h-[600px] w-full">
      <TransitionGroup
        :css="groupAnimated"
        class="flex flex-col justify-center absolute top-[250px] left-1/2 transform -translate-x-1/2 -translate-y-1/2 z-20"
        name="list"
        tag="ul"
      >
        <li
          class="outline card mt-2 w-80 h-32 border rounded-md flex justify-center items-center transition-all"
          :class="
            isMoveInTimeline ? 'opacity-50 bg-yellow-300 relative' : 'opacity-0'
          "
          :style="outlineStyles"
        >
          Input Here?!
        </li>
        <li
          :style="getTimelineStyle(index)"
          ref="timelineCards"
          :class="groupAnimated ? 'transition-all duration-1000' : ''"
          class="card mt-2 w-72 h-28 border rounded-md p-4 flex mx-auto items-center relative"
          :key="cardList.title"
          v-for="(cardList, index) in cardLists"
        >
          <div
            class="absolute top-0 left-1/2 transform -translate-x-1/2 -translate-y-1/2 w-16 h-6 rounded-xl px-2 text-center text-white font-bold z-20"
            :class="
              cardList.isCorrect === null
                ? 'bg-blue-300'
                : cardList.isCorrect
                ? 'bg-green-300'
                : 'bg-red-300'
            "
          >
            {{ cardList.year.split("-")[0] }}
          </div>
          <div class="left w-20 border h-full shrink-0">
            <img :src="cardList.img" class="h-full object-cover" alt="" />
          </div>
          <div class="px-2">
            {{ cardList.title }}
          </div>
        </li>
      </TransitionGroup>
    </div>
    <div
      :class="isMoveInTimeline ? 'h-[720px]' : 'h-[520px]'"
      class="timeline absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 w-1 rounded-md bg-white transition-all z-0"
    ></div>
    <div class="absolute bottom-2">After</div>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
