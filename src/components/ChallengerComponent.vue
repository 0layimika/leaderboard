<template>
 <div style="justify-content: center;">
  <span v-for="challenger in this.params.player.challengers" :key="challenger">
   <span v-if="challenger.inGame" class="unavailable-challenger">{{ challenger.name }}</span>
   <span v-else class="challenger">{{ challenger.name }}</span></span>
 </div>
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
 color: white;
  padding: 0.25rem 0.5rem;
  margin-right: 1px;
  border-right: 1px solid;
  border-left: 1px solid;
  background-color: #319550;
}
.unavailable-challenger{
  margin-right: 1px;
  border-right: 1px solid black;
  border-left: 1px solid black;
  border-top: 1px solid black;
   border-bottom: 1px solid black;
  background-color: white;
  color:rgb(234, 89, 89);
  text-decoration: line-through 1px black;
  padding: 0.25rem 0.5rem;
}
</style>