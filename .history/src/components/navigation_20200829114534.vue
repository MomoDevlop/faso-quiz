<template>
 <b-nav tabs>
      <b-nav-item disabled><b>Fancy quiz app</b> </b-nav-item>
      <b-nav-item disabled>Counter : 4/10 </b-nav-item>
 </b-nav>

</template>

<script>
export default {
     props: [
          'numberCorrect',
          'numTotal'
     ]
}
</script>