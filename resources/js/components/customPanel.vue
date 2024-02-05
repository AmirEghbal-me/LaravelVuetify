<template>
    <div style="width: 1024px;height: 600px;">
        <div ref="draggableContainer" id="draggable-container">
            <div id="draggable-header" @mousedown="dragMouseDown">
                <div style="padding: 5px;width: 600px;box-shadow: 1px 2px 5px 5px rgba(0,0,0,0.29);">
                    <CustomPanelHeader/>
                    <FormValidation/>
                </div>
            </div>
        </div>
    </div>

</template>
<script>
import FormValidation from "../../src/components/FormValidation.vue";
import CustomPanelHeader from "./CustomPanelHeader.vue";
export default {
    components: {CustomPanelHeader, FormValidation},
    data: () => ({
        positions: {
            clientX: undefined,
            clientY: undefined,
            movementX: 0,
            movementY: 0
        },
        panel: true,
    }),
    methods: {
        dragMouseDown: function (event) {
            event.preventDefault()
            // get the mouse cursor position at startup:
            this.positions.clientX = event.clientX
            this.positions.clientY = event.clientY
            document.onmousemove = this.elementDrag
            document.onmouseup = this.closeDragElement
        },
        elementDrag: function (event) {
            event.preventDefault()
            this.positions.movementX = this.positions.clientX - event.clientX
            this.positions.movementY = this.positions.clientY - event.clientY
            this.positions.clientX = event.clientX
            this.positions.clientY = event.clientY
            // set the element's new position:
            this.$refs.draggableContainer.style.top = (this.$refs.draggableContainer.offsetTop - this.positions.movementY) + 'px'
            this.$refs.draggableContainer.style.left = (this.$refs.draggableContainer.offsetLeft - this.positions.movementX) + 'px'
        },
        closeDragElement () {
            document.onmouseup = null
            document.onmousemove = null
        },
    },
}
</script>
<style>
#draggable-container {
    position: absolute;
    z-index: 9;
    width: 100%;
    height: 100%;
}
#draggable-header {
    z-index: 10;
    width: 100%;
    height: 100%;
}
</style>
