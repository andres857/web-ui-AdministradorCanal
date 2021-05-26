<template>
    <b-card
    img-src="https://mediawebwordpress.nyc3.digitaloceanspaces.com/Imbanaco/salasespera/salasespera/Sotano4-radioterapia.jpeg"
    img-alt="Image"
    img-top
    style="max-width: 18rem;"
    >
    <template #header>
      <h4 class="mb-0">Sotano 4</h4>
    </template>
            <b-row align-v="center" align-h="center">
                <b-col cols="1">
                    <b-icon icon="circle-fill" variant="success"></b-icon>
                </b-col>
                <b-col cols="6">
                    <span>Radioterapia 1</span>
                </b-col>
                <b-col cols="1">
                    <b-dropdown size="sm" variant="link" toggle-class="text-decoration-none" no-caret>
                        <template #button-content>
                            <b-icon icon="three-dots-vertical" aria-hidden="true" ></b-icon>
                        </template>
                        <b-dropdown-item>Cambiar Canal</b-dropdown-item>
                        <b-dropdown-item>Reiniciar</b-dropdown-item>
                    </b-dropdown>
                </b-col>
            </b-row>
            <hr>

            <b-row align-v="center" align-h="center">
                <b-col cols="1">
                    <b-icon icon="circle-fill" variant="success"></b-icon>
                </b-col>
                <b-col cols="6">
                    <span>Radioterapia 2</span>
                </b-col>
                <b-col cols="1">
                    <b-dropdown size="sm" variant="link" toggle-class="text-decoration-none" no-caret>
                        <template #button-content>
                            <b-icon icon="three-dots-vertical" aria-hidden="true" ></b-icon>
                        </template>
                        <b-dropdown-item>Cambiar Canal</b-dropdown-item>
                        <b-dropdown-item>Reiniciar</b-dropdown-item>
                    </b-dropdown>
                </b-col>
            </b-row>
            <hr>

            <b-row align-v="center" align-h="center">
                <b-col cols="1">
                    <b-icon icon="circle-fill" variant="success"></b-icon>
                </b-col>
                <b-col cols="6">
                    <span> Medicina Nuclear </span>
                </b-col>
                <b-col cols="1">
                    <b-dropdown size="sm" variant="link" toggle-class="text-decoration-none" no-caret>
                        <template #button-content>
                            <b-icon icon="three-dots-vertical" aria-hidden="true" ></b-icon>
                        </template>
                        <b-dropdown-item>Cambiar Canal</b-dropdown-item>
                        <b-dropdown-item>Reiniciar</b-dropdown-item>
                    </b-dropdown>
                </b-col>
            </b-row>
    </b-card>
</template>

<script>
export default {
    
}
</script>

<style scoped>

</style>