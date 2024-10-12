<script setup>
import QRCode from "qrcode";

const firstName = ref("");
const lastName = ref("");
const phoneNumber = ref("");
const profession = ref("");
const email = ref("");
const city = ref("");
const country = ref("");

const qrCodeUrl = ref(null);

//const canvas=ref('qrcanvas')

const cardInfos = computed(() => {
  return `
BEGIN:VCARD
 VERSION:4.0 
 FN:${firstName.value} ${lastName.value}
 N:${firstName.value};${lastName.value};
 TITLE: ${profession.value}
 TEL: ${phoneNumber.value}
 EMAIL;TYPE=work:${email.value}
 END:VCARD
`;
});

function generateQRCode() {
  console.log(firstName.value);
  //QRCode.toCanvas(canvas, 'sample text', function (error) {
  //if (error) console.error(error)
  //console.log('success!');})
  QRCode.toDataURL(cardInfos.value)
    .then((url) => {
      qrCodeUrl.value = url;
      console.log(url);
    })
    .catch((err) => {
      console.error(err);
    });
}

function downloadQRCode() {
  if (!qrCodeUrl.value) return;
  const link = document.createElement("a");
  link.href = qrCodeUrl.value;
  link.download = "qrcode.png";
  link.click();
}

const inputClass =
  "col-span-2 bg-[#3B3C46] border-2 border-transparent px-3 py-3 text-gray-100 text-[15px] font-[300] rounded-md caret-[#00FCFC] placeholder:text-gray-400 hover:border-purpule-500 [#635983] focus:border-purple-400 [#635983] focus:outline-none";
</script>

<template>
  <div
    class="min-h-screen w-full px-14 flex items-center justify-center bg-[#2B2738] selection:bg-[#009999]"
  >
    <div class="w-full flex gap-10 p-10 rounded-md">
      <div class="flex-1 text-white p-5">
        <p class="text-5xl">Create a vCard</p>
        <p class="text-sm text-gray-300 font-[300] mt-3">
          Already have a vCard with us ?
          <span
            class="text-purple-500 underline underline-offset-2 cursor-pointer hover:text-[#00FCFC] duration-300 ease-in-out"
            >Get my vCard</span
          >
        </p>

        <div class="grid grid-cols-2 gap-4 mt-12">
          <input
            type="text"
            v-model="firstName"
            :class="['!col-span-1', inputClass]"
            placeholder="First Name"
          />
          <input
            type="text"
            v-model="lastName"
            :class="['!col-span-1', inputClass]"
            placeholder="Last Name"
          />
          <input
            type="text"
            v-model="profession"
            :class="inputClass"
            placeholder="Profession"
          />
          <input
            type="text"
            v-model="email"
            :class="inputClass"
            placeholder="E-mail "
          />
          <input
            type="text"
            v-model="phoneNumber"
            :class="['!col-span-1 ', inputClass]"
            placeholder="Phone Numner"
          />
          <input
            type="text"
            v-model="city"
            :class="['!col-span-1', inputClass]"
            placeholder="City"
          />
          <input
            type="text"
            v-model="country"
            :class="inputClass"
            placeholder="Country"
          />
          <button
            @click="generateQRCode"
            class="col-span-2 p-3 mt-3 bg-purple-500 hover:bg-purple-600 rounded-md duration-500 ease-in-out shadow-md active:scale-90 duration-300"
          >
            Genenerate
          </button>
        </div>
      </div>

      <div
        class="relative flex-1 rounded-xl overflow-hidden bg-no-repeat bg-cover bg-center bg-[url('/assets/images/nature-eye.jpg')]"
      >
        <div
          class="absolute w-[220px] py-7 px-5 flex flex-col items-center top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 bg-white/20 rounded-xl shadow-md"
        >
          <div class="size-[150px]" v-if="!qrCodeUrl">
            <img
              src="/assets/images/qr-sample.png"
              alt="qr-sample"
              class="w-full h-full"
            />
          </div>
          <div class="size-[150px]" v-else>
            <img :src="qrCodeUrl" alt="qr-sample" class="w-full h-full" />
          </div>
          <div class="flex w-full justify-center">
            <button
              @click="downloadQRCode"
              :class="[
                !qrCodeUrl && '!bg-gray-400 !text-gray-700 !cursor-not-allowed',
                'w-4/5 p-3 mt-5 bg-white hover:bg-purple-600 font-bold  hover:text-white rounded-md duration-500 ease-in-out shadow-md font-[400] active:scale-90 duration-300',
              ]"
            >
              Download
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
