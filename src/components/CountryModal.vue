<template>
    <div>
        {{ country.cca2 }}
    </div>
    <!-- <div class="modal">
        <div class="modal-content">
            <h2>{{ country.name.official }}</h2>
            <p>2 character Country Code: {{ country.cca2 }}</p>
            <p>3 character Country Code: {{ country.cca3 }}</p>
            <p>Native Country Name: {{ country.name.nativeName }}</p>
            <p>Alternative Country Name: {{ country.altSpellings.join(', ') }}</p>
            <p>Country Calling Codes: {{ country.idd }}</p>
            <button @click="$emit('close')">Close</button>
        </div>
    </div> -->
    <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h1 class="modal-title fs-5" id="exampleModalLabel"></h1>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <div v-if="Object.keys(country).length > 0">
            <p>2 character Country Code: {{ country.cca2 }}</p>
            <p>3 character Country Code: {{ country.cca3 }}</p>
            <p>Native Country Name: {{country.name.official}}</p>
            <p>Alternative Country Name:</p>
            <p>Country Calling Codes: {{ country.idd }}</p>
        </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-outline-secondary" data-bs-dismiss="modal">Close</button>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
    props: ['country']
};
</script>

