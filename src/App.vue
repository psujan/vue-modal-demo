<template>
  <div>
    <header>
      <div class="logo">
        <span>Modal Demo Using Vue 3</span>
        <svg
          class="logo"
          viewBox="0 0 128 128"
          width="24"
          height="24"
          data-v-e0ef77de=""
        >
          <path
            fill="#42b883"
            d="M78.8,10L64,35.4L49.2,10H0l64,110l64-110C128,10,78.8,10,78.8,10z"
            data-v-e0ef77de=""
          ></path>
          <path
            fill="#35495e"
            d="M78.8,10L64,35.4L49.2,10H25.6L64,76l38.4-66H78.8z"
            data-v-e0ef77de=""
          ></path>
        </svg>
      </div>
    </header>
    <div class="container">
      <div class="box">
        <div class="row x-center">
          <div class="row-left">
            <div class="box">
              <div class="form">
                <form @submit.prevent="openModal">
                  <div class="form-input-wrap flx">
                    <label for="animation-type">Dismiss on overlay</label>
                    <input type="checkbox" v-model="form.overlayClose" />
                  </div>
                  <div class="form-input-wrap flx">
                    <label for="animation-type">Width</label>
                    <input type="number" v-model="form.width" />
                  </div>
                  <div class="form-input-wrap">
                    <label for="position">Position</label>
                    <select
                      name="position"
                      id="position"
                      v-model="form.position"
                    >
                      <option value="">Choose Position</option>
                      <option value="center">Center</option>
                      <option value="topLeft">Top Left</option>
                      <option value="topCenter">Top Center</option>
                      <option value="topRight">Top Right</option>
                      <option value="bottomLeft">Bottom Left</option>
                      <option value="bottomCenter">Bottom Center</option>
                      <option value="bottomRight">Bottom Right</option>
                    </select>
                  </div>
                  <div class="form-input-wrap">
                    <label for="animation-type">Animation type</label>
                    <select
                      name="animation_name"
                      id="animation-type"
                      v-model="form.animationName"
                    >
                      <option value="">Choose Animation Type</option>
                      <option value="zoom">Zoom</option>
                      <option value="slideDown">Slide Down</option>
                      <option value="slideUp">Slide Up</option>
                      <option value="slideLeft">Slide Left</option>
                      <option value="slideRight">Slide Right</option>
                    </select>
                  </div>
                  <div class="form-input-wrap flx">
                    <label for="animation-type">Title</label>
                    <input type="text" v-model="form.title" />
                  </div>
                  <div class="form-action flx flx-end">
                    <button class="btn btn-flat" type="button" @click="reset">
                      Reset
                    </button>
                    <button class="btn" type="submit">Open Modal</button>
                  </div>
                </form>
              </div>
            </div>
          </div>
          <div class="row-right">
            <h4>Component Code</h4>
            <pre class="pre">{{ componentHtml }}</pre>
          </div>
        </div>
      </div>
    </div>
    <base-modal
      v-model:modal="showModal"
      :modal-title="form.title"
      :animation-name="form.animationName"
      :position="form.position"
      :width="form.width"
      :overlay-close="form.overlayClose"
    >
      <p>
        Modals are UI blocks intended to grab full user attention. This is
        usefull when you want your user to focus on the content you are trying
        to inform. Common example includes flashing some notices , advertisments
        , handling forms , confirmation actions and so on.
      </p>
    </base-modal>
  </div>
</template>

<script setup>
import BaseModal from "./components/modal/BaseModal.vue";
import { ref, reactive, computed } from "vue";
/**
 * constants
 */
const formFields = {
  overlayClose: true,
  width: 600,
  position: "center",
  animationName: "slideDown",
  title: "Thank you for viewing this modal !! 🙏",
};
/**
 * reactive data
 */
const showModal = ref(false);
const form = reactive({
  ...formFields,
});
/**
 * functions
 */
const openModal = () => {
  showModal.value = true;
};

const reset = () => {
  form.overlayClose = true;
  form.width = 600;
  form.title = "";
  form.animationName = "";
  form.position = 'center';
};

const componentHtml = computed(() => {
  let text;
  text = `<base-modal
    v-model:modal="${showModal.value}"
    modal-title="${form.title}"
    :animation-name="${form.animationName}"
    :position="${form.position}"
    :width="${form.width}"
    :overlay-close="${form.overlayClose}"
  >
    <p>
      Modals are UI blocks intended to grab full user attention. This is
      usefull when you want your user to focus on the content you are trying
      to inform. Common example includes flashing some notices , advertisments
      , handling forms , confirmation actions and so on.
    </p>
  </base-modal>`;
  return text;
});
</script>

<style scoped>
.modal-title {
  font-size: 16px;
  font-weight: 600;
}
.form label {
  display: inline-block;
  margin-right: 8px;
  min-width: 200px;
}

.form input,
select {
  padding: 10px 6px;
  border: 1px solid #ccc;
  outline: none;
  transition: 0.3s ease;
  border-radius: 4px;
  transition: 0.3s ease;
  max-width: 100%;
}

.form input:focus,
select:focus {
  border: 1px solid #2792ef;
}
.form-input-wrap {
  padding: 10px 0;
  margin-bottom: 1rem;
}

.form-action {
  padding: 1rem 0;
  border-top: 1px solid #ccc;
}
.form-action button {
  display: inline-block;
  margin: 0 10px;
}

.row-left {
  border-right: 1px solid #ccc;
  width: 40%;
}

.row-right {
  width: 60%;
}
.pre {
  background: rgb(28, 23, 23);
  padding: 10px;
  color: #fff;
  border-radius: 6px;
  overflow-x: scroll;
}

header {
  padding: 2rem;
  background: #f5f5f5;
  text-align: center;
}

.logo span {
  font-weight: 900;
  font-size: 20px;
}

.modal-btn-group{
  border-top:1px solid #ccc;

}
</style>
