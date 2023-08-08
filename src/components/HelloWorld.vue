<script setup>
import { ref, onMounted, computed } from "vue";

const data = ref([
  {
    puzzle: "2023-08-06",
    date: "",
    introtxt: "",
    start: "",
    start_order: "9",
    order: 4,
    image: "joplin",
    "image link": "Overlooked image?",
    credit: "Hulton Archive/Getty Images (Scott Joplin)",
    year: "1899",
    clue: "Scott Joplin publishes the song “Maple Leaf Rag,” which becomes a national sensation.",
    context:
      'Renowned as the “King of Ragtime,” Joplin was desperate to <a href="https://www.nytimes.com/2023/07/27/arts/music/treemonisha-scott-joplin-productions.html" target="_blank">prove himself</a> by writing “Treemonisha,” a long-forgotten opera that has recently been performed in three countries.',
    url: "",
    excerpt: "",
    undefined: "",
    potientalValue: 5,
    id: "clue-04",
    percent_correct_text: "57%",
  },
  {
    puzzle: "2023-08-06",
    date: "",
    introtxt: "",
    start: "",
    start_order: "8",
    order: 5,
    image: "ivy",
    "image link": "this image in story",
    credit: "Christopher Capozziello for The New York Times (Ivy League)",
    year: "1920s",
    clue: "Ivy League colleges begin legacy admissions, in part to protect Protestants from Catholic and Jewish applicants.",
    context:
      'Alumni of many colleges are <a href="https://www.nytimes.com/2023/07/30/us/politics/legacy-admissions-college-alumni.html" target="_blank">now wrestling</a> with the practice of legacy admissions.',
    url: "Story",
    excerpt: "",
    undefined: "",
    potientalValue: 5,
    id: "clue-05",
    percent_correct_text: "69%",
  },
  {
    puzzle: "2023-08-06",
    date: "",
    introtxt: "",
    start: "",
    start_order: "7",
    order: 7,
    image: "credit",
    "image link": "Timesmachine image?",
    credit: "",
    year: "1974",
    clue: "The Equal Credit Opportunity Act passes, helping women get mortgages and credit cards in their own name.",
    context:
      'Recent shows, podcasts and films <a href="https://www.nytimes.com/2023/07/25/arts/television/dirty-pictures-from-a-revolution.html" target="_blank">have delved</a> into the 1970s, a time of confusions and correspondences between women’s liberation and the sexual revolution.',
    url: "Story",
    excerpt: "Nyt story",
    undefined: "",
    potientalValue: 4,
    id: "clue-07",
    percent_correct_text: "66%",
  },
  {
    puzzle: "2023-08-06",
    date: "",
    introtxt: "",
    start: "",
    start_order: "6",
    order: 6,
    image: "daisy",
    "image link": "screenshot of ad?",
    credit: "",
    year: "1964",
    clue: "The “Daisy” campaign ad airs, invoking the risk of nuclear war to ominous effect.",
    context:
      'After the development of the atomic bomb, nuclear imagery quickly became a source of <a href="https://www.nytimes.com/2023/07/31/movies/oppenheimer-asteroid-city-mushroom-cloud.html" target="_blank">horror, anxiety and satire</a> in American culture.',
    url: "Story",
    excerpt: "",
    undefined: "",
    potientalValue: 4,
    id: "clue-06",
    percent_correct_text: "91%",
  },
  {
    puzzle: "2023-08-06",
    date: "",
    introtxt: "",
    start: "",
    start_order: "5",
    order: 3,
    image: "edison2",
    "image link": "patent image?",
    credit: "National Archives (light bulb)",
    year: "1879",
    clue: "Thomas Edison demonstrates a practical incandescent light bulb, which lasts longer than other versions.",
    context:
      'Before electricity, <a href="https://www.nytimes.com/2011/06/05/magazine/bulb-in-bulb-out.html">light was expensive</a>, a product of exhaustible sources like whale oil. As of last week, consumers will <a href="https://www.nytimes.com/2023/08/01/climate/incandescent-light-bulb-ban-leds.html" target="_blank">no longer</a> be able to purchase incandescent light bulbs.',
    url: "Story",
    excerpt: "Original story, Learning Network, Times Machine, NYT mag",
    undefined: "",
    potientalValue: 3,
    id: "clue-03",
    percent_correct_text: "93%",
    being_dragged: false,
    translate_y: 98,
    placed: true,
    beingPlaced: false,
    currentOrder: 2,
    correctOrder: 2,
    correct: true,
    diffrence: 0,
    pointValue: 3,
    guess: ["1787", "1985"],
    aboveStack: false,
    beingCorrected: false,
  },
  {
    puzzle: "2023-08-06",
    date: "",
    introtxt: "",
    start: "",
    start_order: "4",
    order: 8,
    image: "peewee",
    "image link": "this image in Loupe?",
    credit: "Warner Bros. (“Pee-Wee's Big Adventure”)",
    year: "1985",
    clue: "“Pee-wee’s Big Adventure” debuts, a breakthrough for the actor Paul Reubens and director Tim Burton.",
    context:
      'Reubens <a href="https://www.nytimes.com/2023/07/31/arts/television/paul-reuben-pee-wee-herman-movies-tv-shows.html" target="_blank">reunited with</a> Burton in “The Nightmare Before Christmas” and appeared as the unsettling Prince Gerhardt in “30 Rock.” He died last week.',
    url: "Story",
    excerpt: "",
    undefined: "",
    potientalValue: 3,
    id: "clue-08",
    percent_correct_text: "82%",
    being_dragged: false,
    translate_y: 156,
    placed: true,
    beingPlaced: false,
    currentOrder: 3,
    correctOrder: 3,
    correct: true,
    diffrence: 0,
    pointValue: 3,
    guess: ["1787", "1999"],
    aboveStack: false,
    beingCorrected: false,
  },
  {
    puzzle: "2023-08-06",
    date: "",
    introtxt: "",
    start: "",
    start_order: "3",
    order: 1,
    image: "nero2",
    "image link": "Nero Bust Scoop Asset",
    credit: "Tom Jamieson for The New York Times (Nero)",
    year: "54-68 A.D.",
    clue: "Nero reigns as emperor of Rome, with a (now disputed) reputation for tyranny, debauchery and indulgence.",
    context:
      'In Rome, archaeologists believe they’ve <a href="https://www.nytimes.com/2023/07/30/world/europe/rome-nero-theater.html" target="_blank">discovered a theater</a> built by Nero, himself a musician. ',
    url: "Story",
    excerpt: "",
    undefined: "",
    potientalValue: 2,
    id: "clue-01",
    percent_correct_text: "93%",
    being_dragged: false,
    translate_y: -18,
    placed: true,
    beingPlaced: false,
    currentOrder: 0,
    correctOrder: 0,
    correct: true,
    diffrence: 0,
    pointValue: 2,
    guess: [null, "1787"],
    aboveStack: false,
    beingCorrected: false,
  },
  {
    puzzle: "2023-08-06",
    date: "",
    introtxt: "",
    start: "",
    start_order: "2",
    order: 9,
    image: "soccer3",
    "image link": "story image? or Loupe image?",
    credit: "Anacleto Rapping/Los Angeles Times, via Associated Press (soccer)",
    year: "1999",
    clue: "The U.S. national women’s soccer team wins its second World Cup, spurring the rapid growth of the sport in America.",
    context:
      'Millions tuned in to watch the United States win in a <a href="https://www.nytimes.com/1999/07/11/sports/refusing-to-wilt-us-wins-soccer-title.html" target="_blank">penalty shootout</a> at the Rose Bowl. The 2023 Women’s World Cup is now in the knockout stages; the United States <a href="https://www.nytimes.com/live/2023/08/06/sports/womens-world-cup-uswnt-sweden-score" target="_blank">lost to Sweden</a> in a penalty shootout in the round of 16 on Sunday.',
    url: "Story",
    excerpt: "",
    undefined: "",
    potientalValue: 2,
    id: "clue-09",
    percent_correct_text: "87%",
    being_dragged: false,
    translate_y: 214,
    placed: true,
    beingPlaced: false,
    currentOrder: 4,
    correctOrder: 4,
    correct: false,
    diffrence: 1,
    pointValue: 0,
    beingCorrected: false,
    guess: [null, "1787"],
    aboveStack: false,
  },
  {
    puzzle: "2023-08-06",
    date: "",
    introtxt: "",
    start: "TRUE",
    start_order: "1",
    order: 2,
    image: "constitution2",
    "image link": "image in story",
    credit: "Ardon Bar-Hama (Constitution)",
    year: "1787",
    clue: "The U.S. Constitution is signed and first printed after four months of grueling debate in Philadelphia.",
    context:
      'Only 14 known copies of the first printing remain. One was sold in 2021 for a record $43.2 million by Dorothy Tapper Goldman, a <a href="https://www.nytimes.com/2023/07/31/nyregion/dorothy-tapper-goldman-dead.html" target="_blank">major collector</a> of American historical documents, who died in July.',
    url: "Story",
    excerpt: "",
    undefined: "",
    id: "clue-02",
    placed: true,
    currentOrder: 1,
    percent_correct_text: "33%",
    correctOrder: 1,
    beingPlaced: false,
    aboveStack: false,
    translate_y: 40,
    beingCorrected: false,
  },
]);
const card = ref(null);
const draggingStyle = ref({});

const currentStep = ref(0);

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
  };

  moveAt(event.pageX, event.pageY);

  const dragmove_handler = (ev) => {
    moveAt(ev.pageX, ev.pageY);
  };
  document.addEventListener("mousemove", dragmove_handler);
  card.value.addEventListener("mouseup", () => {
    document.removeEventListener("mousemove", dragmove_handler);
    card.value.removeEventListener("mouseup", () => {});
  });
};

const currentDraggingStyle = computed({
  get: () => {
    return draggingStyle.value;
  },
  set: (val) => {
    console.log(val);
    draggingStyle.value = val;
  },
});

onMounted(() => {
  card.value.addEventListener("mousedown", dragstart_handler);
  card.value.addEventListener("dragstart", () => {
    return false;
  });
});

defineProps({
  msg: String,
});

const count = ref(0);
</script>

<template>
  <div class="scoreboard-container flex">
    <div>{{ currentStep }} of {{ data.length }}</div>
    <div>
      <ul class="flex m-2">
        <li
          class="w-10 h-2 rounded-md border mr-2 flex items-center justify-center"
          v-for="step in data.length"
        >
          {{ step }}
        </li>
      </ul>
    </div>
    <div>8 points</div>
  </div>
  <div class="current-card flex justify-center mt-10 relative h-32">
    <div
      ref="card"
      class="card w-80 h-32 border rounded-md p-4 flex cursor-grab"
      :style="currentDraggingStyle"
    >
      <div class="left w-32 border h-full"></div>
      <div class="w-60 px-4">Draggable</div>
    </div>
  </div>
  <div class="timeline flex flex-col items-center my-20 relative">
    <div class="absolute top-[-30px]">Before</div>

    <ul class="min-h-[200px] flex flex-col justify-center">
      <li
        class="card mt-2 w-72 h-28 border rounded-md p-4 flex"
        v-for="step in 1"
      ></li>
      <li
        class="timeline absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 w-2 h-full rounded-md bg-white"
      ></li>
    </ul>
    <div class="absolute bottom-[-30px]">After</div>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
