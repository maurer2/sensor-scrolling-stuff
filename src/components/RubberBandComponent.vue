<template>
    <section class="slide-wrapper" @pointerdown="activatePointer" @pointerup="deactivatePointer" @pointermove="pointerMove"
             @pointerleave="deactivatePointer">
        <figure class="slide" :style="styleAttribute" ref="slide">
            <img src="../assets/cat3-small.jpg" alt="background-image" class="slide-background slide-background--is-slideable" ref="image">
            <figcaption class="slide-content">
                Meow!
            </figcaption>
        </figure>
    </section>
</template>

<script lang="ts">
    import { Component, Vue } from 'vue-property-decorator';

    @Component
    export default class RubberBandComponent extends Vue {
        private pointerIsDown: boolean = false;
        private translateX: number = 0;
        private transitionDuration: number = 0;

        private activatePointer() {
            this.pointerIsDown = true;
        }

        private deactivatePointer() {
            this.resetTransition();
            this.pointerIsDown = false;
        }

        private pointerMove(event: PointerEvent) {
            if (!this.pointerIsDown) {
                return
            }

            this.activateTransition(event.x);
        }

        private activateTransition(pointerX: number) {
            const container = this.$refs.slide as HTMLElement;
            const image = this.$refs.image as HTMLElement;

            const boundingBoxContainer = container.getBoundingClientRect();
            const width = boundingBoxContainer.width;
            const offsetLeft = container.offsetLeft;

            const overflowSize = Math.abs(image.offsetLeft);

            const currentPositionInPercent = Math.round((pointerX - offsetLeft) * 100 / width);
            const maxTransformX = Math.round(overflowSize * 100 / width);
            const elongationFromCenterAbsolute = (currentPositionInPercent - 50) * 2;

            //slightly long variable name
            const elongationFromCenterMappedToOverflowSize = (maxTransformX * elongationFromCenterAbsolute) / 100;

            this.transitionDuration = 0;
            this.translateX = elongationFromCenterMappedToOverflowSize;
        }

        private resetTransition() {
            this.translateX = 0;
            this.transitionDuration = 500;
        }

        get styleAttribute() {
            return {
                transform: `translateX(${this.translateX}%)`,
                transitionDuration: `${this.transitionDuration}ms`
            };
        }
    }
</script>

<style scoped lang="scss">
    .slide-wrapper {
        margin: 2rem 0;
        padding: 0;
        background: aliceblue;
        overflow: hidden;
    }

    .slide {
        position: relative;
        margin: 0;
        padding: 0;
        transform: translateX(0);
        will-change: transform;
        touch-action: none;
        transition: transform;
        user-select: none;
    }

    .slide-background {
        display: block;
        width: 100%;
        height: auto;
        pointer-events: none;

        &--is-slideable {
            $width: 200%;
            $overflow-width: $width - 100%;

            width: $width;
            margin-left: ($overflow-width / 2) * -1;
            margin-right: ($overflow-width / 2) * -1;
        }
    }

    .slide-content {
        position: absolute;
        left: 1rem;
        right: 1rem;
        bottom: 1rem;
        text-align: center;
        background: #fff;
    }
</style>
