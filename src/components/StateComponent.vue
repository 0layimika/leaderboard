<template>
 <div v-if="this.params.player.inGame" style="align-self: center; padding-top: 7px;">
  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="green">
   <path
    d="M7.75194 5.43872L18.2596 11.5682C18.4981 11.7073 18.5787 12.0135 18.4396 12.252C18.3961 12.3265 18.3341 12.3885 18.2596 12.432L7.75194 18.5615C7.51341 18.7006 7.20725 18.62 7.06811 18.3815C7.0235 18.305 7 18.2181 7 18.1296V5.87061C7 5.59446 7.22386 5.37061 7.5 5.37061C7.58853 5.37061 7.67547 5.39411 7.75194 5.43872Z">
   </path>
  </svg>
 </div>
 <div v-if="false" style="align-self: center; padding-top: 7px;">
  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="#ED7014">
   <path
    d="M6 5H18C18.5523 5 19 5.44772 19 6V18C19 18.5523 18.5523 19 18 19H6C5.44772 19 5 18.5523 5 18V6C5 5.44772 5.44772 5 6 5Z">
   </path>
  </svg>
 </div>
 <div v-else style="align-self: center; padding-top: 7px;">
  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="orange">
   <path
    d="M12 22C6.47715 22 2 17.5228 2 12C2 6.47715 6.47715 2 12 2C17.5228 2 22 6.47715 22 12C22 17.5228 17.5228 22 12 22ZM9 9V15H11V9H9ZM13 9V15H15V9H13Z">
   </path>
  </svg>
 </div>
</template>
