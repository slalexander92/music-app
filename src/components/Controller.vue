<template>

  <div id="controller">
    <i class="material-icons">skip_previous</i>
    <i class="material-icons">play_arrow</i>
    <i class="material-icons">pause</i>
    <i class="material-icons">skip_next</i>
  </div>

</template>

<script>
export default {

  data:() => ({

  }),

}
</script>

<style lang="scss" scoped>

@import '../globals/variables';

#controller{

}

i.material-icons{
  cursor: pointer;
  color: $green;
  font-size: 61px;

  &:hover{
    color:$hoverGreen;
  }
}

</style>
