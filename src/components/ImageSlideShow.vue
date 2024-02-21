<template>
    <div>
        <header>
            <HeaderComponent />
        </header>

        <main>
            <BodyComponent />

            <div class="elfsight" data-elfsight-app-lazy></div>

            <!-- Your additional content here -->

            <div class="images">

                <Flicking :options="{ circular: false }" :plugins="plugins">
                    <div class="card-panel">
                        <img alt="car logo" class="car1" src="@/assets/Images/Home1.jpg" width="400" height="600" />
                    </div>
                    <div class="card-panel">
                        <img alt="car logo" class="car2" src="@/assets/Images/Home2.jpg" width="400" height="600" />
                    </div>
                    <div class="card-panel">
                        <img alt="car logo" class="car3" src="@/assets/Images/Home3.jpg" width="400" height="600" />
                    </div>
                    <template #viewport>
                        <span class="flicking-arrow-prev is-thin"></span>
                        <span class="flicking-arrow-next is-thin"></span>
                    </template>


                </Flicking>
                <!-- End of additional content -->
            </div>
            <div class="background">
                <img alt="car logo" class="background" src="@/assets/Images/background1.jpg" />

            </div>

        </main>

        <footer>
            <FooterComponent />
        </footer>
    </div>
</template>

<script>
    import HeaderComponent from "@/components/HeaderComponent.vue";
    import FooterComponent from "@/components/FooterComponent.vue";
    import BodyComponent from "@/components/BodyComponent.vue";
    import Flicking from "@egjs/vue3-flicking";
    import { Arrow } from "@egjs/flicking-plugins";
    import "@egjs/flicking-plugins/dist/arrow.css";

    export default {
        components: {
            HeaderComponent,
            FooterComponent,
            BodyComponent,
            Flicking,
        },
        data() {
            return {
                plugins: [new Arrow()],
            };
        },
    };
</script>

<style>
    .background {
        position: absolute;
        width: 160vh;
        height: 600px;
        left: 0;
        z-index: -30;
    }

    .images {
        position: absolute;
        right: 0;
        z-index: -1;
        width: 400px;

    }

    .elfsight {
        position: absolute;
       top:0;
       z-index:12;
    }
</style>
