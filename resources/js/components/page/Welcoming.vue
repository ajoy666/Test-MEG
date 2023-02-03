<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">Azi Fauzi</div>

                    <div class="card-body">
                        Test Programming
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    mounted() {
        this.$Progress.start()
        console.log('Component mounted.')
        this.$Progress.finish()

    }
}
</script>
