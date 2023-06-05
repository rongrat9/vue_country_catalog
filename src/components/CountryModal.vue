<template>
    <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h1 class="modal-title fs-5" id="exampleModalLabel">
                        <div v-if="Object.keys(country).length > 0">
                            {{ country.name.official }}
                        </div>
                    </h1>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div v-if="Object.keys(country).length > 0" class="row">
                        <div class="col-4 text-start"><p class="fw-bold">CCA2:</p></div>
                        <div class="col-8 text-start">{{ country.cca2 }}</div>
                        <div class="col-4 text-start"><p class="fw-bold text-start">CCA3:</p></div>
                        <div class="col-8 text-start">{{ country.cca3 }}</div>
                        <div class="col-4 text-start"><p class="fw-bold text-start">Alternative Name:</p></div>
                        <div class="col-8 text-start">{{ country.altSpellings.join('| ') }}</div>
                        <div class="col-4 text-start"><p class="fw-bold text-start">Calling Code:</p></div>
                        <div class="col-8 text-start">{{ country.idd.root }}</div>
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

