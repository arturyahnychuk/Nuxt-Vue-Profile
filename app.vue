<template>
  <div class="w-full max-w-[560px] mx-auto py-16">
    <h2 class="text-base font-semibold leading-7 text-gray-900">Profile</h2>
    <p class="mt-1 text-sm leading-6 text-gray-600 mb-4">
      This information will be displayed publicly so be careful what you share.
    </p>
    <form @submit.prevent="submitForm" ref="form" class="space-y-8">

      <div>
        <label
          for="website-url"
          class="block text-sm font-medium leading-6 text-gray-900"
          >Website URL</label
        >
        <div
          class="mt-2 flex rounded-md shadow-sm ring-1 ring-inset ring-gray-300 focus-within:ring-2 focus-within:ring-inset focus-within:ring-indigo-600"
        >
          <input @input="onChange"
            type="url"
            name="website-url"
            id="website-url"
            v-model.trim="formData.websiteUrl"
            class="block flex-1 border-0 bg-transparent h-[36px] !px-3 outline-none py-1.5 pl-1 text-gray-900 placeholder:text-gray-400 focus:ring-0 sm:text-sm sm:leading-6"
            placeholder="Enter website URL"
            @blur="showNextQuestion('projectType')"
            required
          />
        </div>
      </div>

      <div v-if="showProjectType && formData.websiteUrl" class="animation">
        <label
          for="project-type"
          class="block text-sm font-medium leading-6 text-gray-900"
          >Type of project</label
        >
        <div
          class="mt-2 flex rounded-md shadow-sm ring-1 ring-inset ring-gray-300 focus-within:ring-2 focus-within:ring-inset focus-within:ring-indigo-600"
        >
          <select
            id="project-type"
            v-model="formData.projectType"
            class="block flex-1 border-0 bg-transparent h-[36px] !px-3 outline-none py-1.5 pl-1 text-gray-900 placeholder:text-gray-400 focus:ring-0 sm:text-sm sm:leading-6"
            @change="showNextQuestion('staticSiteType')"
            required
          >
            <option value="Open Source Library">Open Source Library</option>
            <option value="SDK">SDK</option>
            <option value="Dev Tools">Dev Tools</option>
            <option value="Other">Other</option>
          </select>
        </div>
      </div>

      <div v-if="showStaticSiteType && formData.projectType" class="animation">
        <label
          for="static-site-type"
          class="block text-sm font-medium leading-6 text-gray-900"
          >Static site type</label
        >
        <div
          class="mt-2 flex rounded-md shadow-sm ring-1 ring-inset ring-gray-300 focus-within:ring-2 focus-within:ring-inset focus-within:ring-indigo-600"
        >
          <select
            id="static-site-type"
            v-model="formData.staticSiteType"
            class="block flex-1 border-0 bg-transparent h-[36px] !px-3 outline-none py-1.5 pl-1 text-gray-900 placeholder:text-gray-400 focus:ring-0 sm:text-sm sm:leading-6"
            @change="handleStaticSiteTypeChange"
            required
          >
            <option value="Sphinx">Sphinx</option>
            <option value="MkDocs">MkDocs</option>
            <option value="Jekyll">Jekyll</option>
            <option value="Docusaurus">Docusaurus</option>
            <option value="Hugo">Hugo</option>
            <option value="Gatsby">Gatsby</option>
            <option value="VuePress">VuePress</option>
            <option value="GitBook (Legacy)">GitBook (Legacy)</option>
            <option value="Next.js">Next.js</option>
            <option value="Other">Other content management system</option>
          </select>
        </div>
      </div>

      <div v-if="showOtherStaticSiteType" class="animation">
        <label
          for="other-static-site-type"
          class="block text-sm font-medium leading-6 text-gray-900"
          >Other Content Management System</label
        >
        <div
          class="mt-2 flex rounded-md shadow-sm ring-1 ring-inset ring-gray-300 focus-within:ring-2 focus-within:ring-inset focus-within:ring-indigo-600"
        >
          <input @input="onChange"
            type="text"
            id="other-static-site-type"
            v-model.trim="otherStaticSiteType"
            class="block flex-1 border-0 bg-transparent h-[36px] !px-3 outline-none py-1.5 pl-1 text-gray-900 placeholder:text-gray-400 focus:ring-0 sm:text-sm sm:leading-6"
            placeholder="Enter other CMS"
            @blur="handleOtherStaticSiteBlur"
            required
          />
        </div>
      </div>

      <div v-if="showEmail && formData.staticSiteType" class="animation">
        <label
          for="email"
          class="block text-sm font-medium leading-6 text-gray-900"
          >Email address</label
        >
        <div
          class="mt-2 flex rounded-md shadow-sm ring-1 ring-inset ring-gray-300 focus-within:ring-2 focus-within:ring-inset focus-within:ring-indigo-600"
        >
          <input @input="onChange"
            type="email"
            name="email"
            id="email"
            v-model.trim="formData.email"
            autocomplete="email"
            class="block flex-1 border-0 bg-transparent h-[36px] !px-3 outline-none py-1.5 pl-1 text-gray-900 placeholder:text-gray-400 focus:ring-0 sm:text-sm sm:leading-6"
            placeholder="Enter email address"
            required
          />
        </div>
      </div>

      <div v-if="showSubmitMessage">
        <p class="text-red-500">All fields are required.</p>
      </div>

      <button
        type="submit"
        class="rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:ring-2 focus-visible:ring-offset-2 focus-visible:ring-indigo-600"
      >
        Submit
      </button>
    </form>
  </div>
</template>

<script setup>
import { ref } from "vue";

const showWebsiteUrl = ref(true);
const showProjectType = ref(false);
const showStaticSiteType = ref(false);
const showEmail = ref(false);
const showSubmitMessage = ref(false);
const showOtherStaticSiteType = ref(false);
const otherStaticSiteType = ref("");

const formData = ref({
  websiteUrl: "",
  projectType: "",
  staticSiteType: "",
  email: "",
});

const onChange = ()=> {
  if (!formData.value.websiteUrl) {
    showProjectType.value = false
    showEmail.value = false
    showStaticSiteType.value = false
    showOtherStaticSiteType.value = false
  }
}

const showNextQuestion = (nextQuestion) => {
  if (nextQuestion === "projectType" && formData.value.websiteUrl) {
    showProjectType.value = true;
  }

  if (nextQuestion === "staticSiteType" && formData.value.projectType) {
    showStaticSiteType.value = true;
  }

  if (nextQuestion === "email" && formData.value.staticSiteType) {
    showEmail.value = true;
  }

};

const handleStaticSiteTypeChange = () => {
  if (formData.value.staticSiteType === "Other") {
    showOtherStaticSiteType.value = true;
  } else {
    showOtherStaticSiteType.value = false;
    showNextQuestion("email");
  }
};

const handleOtherStaticSiteBlur = () => {
  if (otherStaticSiteType.value) {
    showNextQuestion("email");
  }
};

const submitForm = () => {
  if (
    !formData.value.websiteUrl ||
    !formData.value.projectType ||
    (!formData.value.staticSiteType && !otherStaticSiteType.value) ||
    !formData.value.email
  ) {
    showSubmitMessage.value = true;
  } else {
    // Reset submission message
    showSubmitMessage.value = false;

    // Form data is valid, handle submission (e.g., send to server)
    console.log("Form submitted:", formData.value);
  }
};
</script>

<style scoped>
/* Add Tailwind CSS classes here if needed */
</style>
