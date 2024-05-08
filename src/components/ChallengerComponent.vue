<template>
 <div>
  <span v-for="challenger in this.params.player.challengers" :key="challenger">
   <span v-if="challenger.inGame" class="unavailable-challenger">{{ challenger.name }}</span>
   <span v-else class="challenger">{{ challenger.name }}</span></span>
 </div>
 {{ this.params.player }}
</template>

<script>
export default {
 methods: {
  // getPlayer(challenger) {
  //  return this.params.players.find(player => player.name === challenger)
  // }
 }
}
</script>

<style>
.challenger{
 margin-right: 3px;
 border-right: 1px solid;
 border-left: 1px solid;
 background-color: greenyellow;
 font-weight: bold;
}
.unavailable-challenger{
  margin-right: 3px;
  border-right: 1px solid black;
  border-left: 1px solid black;
  background-color: white;
  color:red;
  text-decoration: line-through 2px black;
}
</style>