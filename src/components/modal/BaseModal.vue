<template>
  <Transition name="modal-fade">
    <div
      :class="['modal-dialog', modal ? 'modal-dialog-active' : '', modalClass]"
      v-show="modal"
      @click="handleOverlayClick"
    >
      <div :class="['modal-wrap', computedPositionClass]">
        <Transition :name="`modal-${animationName}`">
          <div
            :class="[modalContentClass]"
            v-show="modal"
            :style="{ width: width + 'px' }"
          >
            <template v-if="useModalHeader">
              <div class="modal-header" :class="[modalHeaderClass]">
                <h4 class="modal-title">
                  <span v-if="modalTitle">{{ modalTitle }}</span>
                </h4>
                <button
                  @click="closeModal"
                  v-if="showCloseIcon"
                  class="modal-close-btn"
                >
                  x
                </button>
              </div>
            </template>
            <slot name="modal-header" />
            <div class="modal-body">
              <slot />
            </div>
            <div class="modal-footer" v-if="useModalFooter">
              <button class="btn" @click="closeModal">Ok!</button>
              <button class="btn btn-flat" @click="closeModal">Cancel</button>
            </div>
            <slot name="modal-footer" />
          </div>
        </Transition>
      </div>
    </div>
  </Transition>
</template>

<script setup>
import { computed } from "vue";
/**
 * props
 */
const props = defineProps({
  overlayClose: {
    type: Boolean,
    default: true,
  },
  modalTitle: {
    type: String,
    default: "",
  },
  modal: {
    type: Boolean,
    default: true,
  },
  useModalHeader: {
    type: Boolean,
    default: true,
  },
  useModalFooter: {
    type: Boolean,
    default: true,
  },
  showCloseIcon: {
    type: Boolean,
    default: true,
  },
  modalHeaderClass: {
    type: String,
    required: false,
  },
  modalContentClass: {
    type: String,
    required: false,
    default: "modal-content",
  },
  modalClass: {
    type: String,
    required: false,
  },
  width: {
    type: Number,
    default: 600,
  },
  animationName: {
    type: String,
    default: "slideDown",
  },
  position: {
    type: String,
    default: "center",
  },
});
/**
 * constants
 */
const positionClass = {
  center: "modalCenter",
  topCenter: "modalTopCenter",
  topLeft: "modalTopLeft",
  topRight: "modalTopRight",
  bottomLeft: "modalBottomLeft",
  bottomCenter: "modalBottomCenter",
  bottomRight: "modalBottomRight",
};
/**
 * emits
 */
const emit = defineEmits(["onClose", "update:modal"]);
/**
 * computed properties
 */
const computedPositionClass = computed(() => {
  return positionClass[props.position];
});
/**
 * functions
 */
const closeModal = () => {
  emit("update:modal", false);
  emit("onClose");
};

const overlayClose = (event) => {
  if (event.target.className.includes("modal-wrap") && props.overlayClose) {
    return true;
  }
  return false;
};
const handleOverlayClick = (event) => {
  if (overlayClose(event)) {
    closeModal();
  }
  return false;
};
</script>

<style scoped>
.modal-dialog {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 999;
  width: 100%;
  height: 100%;
  outline: none;
  pointer-events: none;
  background: rgba(0, 0, 0, 0.3);
}
.modal-dialog-active {
  pointer-events: auto;
}
.modal-header,
.modal-body,
.modal-footer {
  padding: 1rem;
}
.modal-header {
  display: flex;
  justify-content: space-between;
}
.modal-title {
  font-size: 1.25rem;
  font-weight: 600;
}
.modal-title span {
  font-size: inherit;
  font-weight: inherit;
}
.modal-close-btn {
  background: #fff;
  outline: none;
  border-radius: 50%;
  border: 0;
  font-size: 1.5rem;
  width: 1.8rem;
  height: 1.8rem;
  transition: background-color 0.3s ease;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal-close-btn:hover {
  background-color: #f1f4f3;
}
.modal-container {
  max-width: 800px;
  height: 100%;
  margin: 0 auto;
}

.modal-footer {
  display: flex;
  align-items: center;
  justify-content: flex-end;
}
.modal-footer button {
  margin: 0 6px;
}
@media (max-width: 768px) {
  .modal-content {
    max-width: 600px;
  }
}
@media (max-width: 500px) {
  .modal-content {
    max-width: 450px;
  }
}
/**Modal Transitions */
.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
}
.modal-fade-enter-to,
.modal-fade-leave-from {
  opacity: 1;
}
.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.3s ease-in-out;
}
/**Effect : Zoom */
.modal-zoom-enter-active {
  animation: zoom 0.3s ease-in-out;
}
.modal-zoom-leave-active {
  animation: zoom 0.3s ease-in-out reverse;
}

@keyframes zoom {
  0% {
    transform: scale(0);
  }

  100% {
    transform: scale(1);
  }
}
/**Effect : Slide Down */
.modal-slideDown-enter-from {
  transform: translateY(-100px);
}
.modal-slideDown-enter-to {
  transform: translateY(0);
}
.modal-slideDown-enter-active {
  transition: transform 0.3s ease-in-out;
}
.modal-slideDown-leave-from {
  transform: translateY(0);
}
.modal-slideDown-leave-to {
  transform: translateY(-100px);
}
.modal-slideDown-leave-active {
  transition: transform 0.3s ease-in-out;
}

/**Effect : Slide Up */
.modal-slideUp-enter-from {
  transform: translateY(100px);
}
.modal-slideUp-enter-to {
  transform: translateY(0);
}
.modal-slideUp-enter-active {
  transition: transform 0.3s ease-in-out;
}
.modal-slideUp-leave-from {
  transform: translateY(0);
}
.modal-slideUp-leave-to {
  transform: translateY(100px);
}
.modal-slideUp-leave-active {
  transition: transform 0.3s ease-in-out;
}

/**Effect : Slide Left */
.modal-slideLeft-enter-from {
  transform: translateX(-100px);
}
.modal-slideLeft-enter-to {
  transform: translateX(0);
}
.modal-slideLeft-enter-active {
  transition: transform 0.3s ease-in-out;
}
.modal-slideLeft-leave-from {
  transform: translateX(0);
}
.modal-slideLeft-leave-to {
  transform: translateX(-100px);
}
.modal-slideLeft-leave-active {
  transition: transform 0.3s ease-in-out;
}

/**Effect : Slide Right */
.modal-slideRight-enter-from {
  transform: translateX(100px);
}
.modal-slideRight-enter-to {
  transform: translateX(0);
}
.modal-slideRight-enter-active {
  transition: transform 0.3s ease-in-out;
}
.modal-slideRight-leave-from {
  transform: translateX(0);
}
.modal-slideRight-leave-to {
  transform: translateX(100px);
}
.modal-slideRight-leave-active {
  transition: transform 0.3s ease-in-out;
}
</style>

<style>
.modalCenter {
  justify-content: center;
  align-items: center;
}
.modalTopLeft {
  justify-content: start !important;
  align-items: flex-start !important;
}
.modalTopCenter {
  justify-content: center !important;
  align-items: flex-start !important;
}
.modalTopRight {
  justify-content: end !important;
  align-items: flex-start !important;
}
.modalBottomLeft {
  justify-content: start;
  align-items: flex-end;
}
.modalBottomCenter {
  justify-content: center;
  align-items: flex-end;
}
.modalBottomRight {
  justify-content: end;
  align-items: flex-end;
}
.modal-wrap {
  display: flex;
  width: 100vw;
  height: 100%;
}
.modal-content {
  position: relative;
  width: 800px;
  background: #fff;
  border-radius: 6px;
  z-index: inherit;
  max-height: 90%;
  overflow-y: auto;
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 12px 0px;
}
</style>
