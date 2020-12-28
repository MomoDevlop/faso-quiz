<template>
 <b-nav tabs class="text-center">
      <b-nav-item disabled><b> <strong>Faso Quiz</strong>  </b> </b-nav-item>
      <b-nav-item disabled>Compteur : {{index + 1}} / 10 </b-nav-item>
 </b-nav>

</template>

<script>
export default {
     props: [
          'numCorrect',
          'index',
          'numTotalAnswered'
     ]
}
</script>