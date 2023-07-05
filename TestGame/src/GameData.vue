<script lang="ts">

export enum Biome {
  Forest,
  Desert,
  Ruins
}

const genericBiomePrefixNames = ["Shadowbane", "Seizing", "Martyr", "Stonehide", "Cogwell", "Petrified", "Voidtouched"]
const genericBiomeSuffixNames = ["of Doom", "of Mystery", "of Antimony", "of Shadows", "of the Kinslayers", "of Draugr"]

const forestMainNames = ["Forest", "Woods", "Canopy", "Treeshroud", "Grove", "Undergrowth", "Weald", "Wood", "Copse", "Woodland", "Thicket"]
const desertMainNames = ["Desert", "Wilds", "Desolace", "Barrens", "Flats", "Dunes", "Wildland", "Scorchland", "Sandpits", "Sandwell", "Reach"]
const ruinsMainNames = ["Ruins", "Remnants", "Rubble", "Wreckage", "Ashland", "Remains", "Hold", "Stockade"]

export const generateBiomeName = (biome: Biome) => {
  let prefix = "";
  let mainName = "";
  let suffix = "";

  if (Math.random() >= 0.25) {
    prefix = selectRandomElementFromArray(genericBiomePrefixNames);
  }
  if (Math.random() >= 0.60) {
    suffix = selectRandomElementFromArray(genericBiomeSuffixNames);
  }
  else if (prefix === "") {
    prefix = selectRandomElementFromArray(genericBiomePrefixNames);
  }
  let arrayToSelectFrom: string[] = []
  switch (biome) {
    case Biome.Forest:
      arrayToSelectFrom = forestMainNames;
      break;
    case Biome.Desert:
      arrayToSelectFrom = desertMainNames;
      break;
    case Biome.Ruins:
      arrayToSelectFrom = ruinsMainNames;
      break;
  }
  mainName = selectRandomElementFromArray(arrayToSelectFrom);
  return `${prefix} ${mainName} ${suffix}`;
}

const selectRandomElementFromArray = (array: Array<string>) => {
  return array[Math.floor(Math.random() * array.length)];
}

export enum EquipmentSlot {
  Head,
  Chest,
  Legs,
  Boots
}
  
export enum ItemRank {
  Basic,
  Improved,
  Epic
}
  
export class Equipment {
  name: String;
  slot: EquipmentSlot;
  
  constructor(iLevel: Number, itemRank: ItemRank) {
    this.name = "Test";
    this.slot = EquipmentSlot.Head;
  }
}

</script>
