<template>
    <b-card
    img-src="https://mediawebwordpress.nyc3.digitaloceanspaces.com/Imbanaco/salasespera/salasespera/Sotano4-radioterapia.jpeg"
    img-alt="Image"
    img-top
    class="MD_cont_imgCard"
    >
    <template #header>
      <h4 class="mb-0 MD_TTCard">Sotano 4</h4>
    </template>
            <b-row class="MD_card">
                <b-col class="MD_contCardInfo">
                    <b-icon icon="circle-fill" variant="success" font-scale="0.7"></b-icon>
                    <span>Radioterapia 1</span>
                </b-col>
                <b-col cols="1" class="MD_contPuntos">
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

            <b-row class="MD_card">

                <b-col class="MD_contCardInfo">
                    <b-icon icon="circle-fill" variant="success" font-scale="0.7"></b-icon>
                    <span>Radioterapia 2</span>
                </b-col>
                <b-col cols="1" class="MD_contPuntos">
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

            <b-row class="MD_card">
                <b-col class="MD_contCardInfo">
                    <b-icon icon="circle-fill" variant="success" font-scale="0.7"></b-icon>
                    <span> Medicina Nuclear </span>
                </b-col>
                <b-col cols="1" class="MD_contPuntos">
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
.MD_cont_imgCard img {
    height: 130px;
    width: max-content;
    margin: 0 auto;
    border-radius: 5px 5px 0 0;
}

.MD_cont_imgCard {
    max-width: 96%;
    margin: 0 auto;
    border-radius: 5px;
}

.MD_cont_imgCard .card-header {
    padding: 4px 16px;
    background: #00B2A9;
}

.MD_cont_imgCard .card-header h4{
    font-size: 20px;
    font-weight: 700;
    color: #ffffff;
}

.MD_card{
    margin: 0;
    justify-content: space-between;
}

.MD_contCardInfo{
    padding: 0;
}

.MD_contCardInfo svg{
    margin-right: 10px;
}

div.card-body {
    padding: 11px 19px;
}

.MD_contPuntos{
    padding: 0;
    width: 28px;
    height: 24px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 60px;
    background: #E6F1F1;
    transition: all 0.4s;
}

.MD_contPuntos svg.b-icon{
    fill: #00B2A9;
    transition: all 0.4s;
}

.MD_contPuntos:hover{
    background: #00B2A9;
}

.MD_contPuntos:hover svg.b-icon{
    fill: #ffffff;
}
</style>