<template>
    <section id="itinerary" class="bg-[#1E1B4B] py-12">
        <div class="container mx-auto px-8">
            <h1 class="text-center text-white text-4xl font-bold mb-12">Agenda</h1>      
            <div class="grid grid-cols-2 gap-12 max-w-screen-lg mx-auto">
            <!-- Day 1 Timeline -->
            <div>
              <h2 class="text-2xl font-medium text-white dark:text-white mb-2 text-center">Day 1</h2>
              <h3 class="text-xl font-italic text-white dark:text-white mb-6 text-center">2 Jan 2025</h3>
              <ol class="relative border-s border-gray-200">                  
                <li class="mb-10 ms-4">
                  <div class="absolute w-3 h-3 bg-gray-200 rounded-full mt-1.5 -start-1.5 border border-white"></div>
                  <time class="mb-1 text-sm font-normal leading-none text-white">10.00 AM</time>
                  <h3 class="text-lg font-semibold text-orange-500">Opening Ceremony</h3>
                  <p class="mb-2 text-base font-thin text-white">Speeches from the Project Director, Advisor of CS Society, Dean of School of Computer Sciences & Platinum Sponsor</p>
                  <p class="mb-2 text-base font-thin text-white">Company Appreciation</p>
                  <p class="mb-2 text-base font-thin text-white">Best Intern Award Ceremony</p>
                </li>
                <li class="mb-10 ms-4">
                  <div class="absolute w-3 h-3 bg-gray-200 rounded-full mt-1.5 -start-1.5 border border-white"></div>
                  <time class="mb-1 text-sm font-normal leading-none text-white">11.00 AM</time>
                  <h3 class="text-lg font-semibold text-orange-500">Exhibitions</h3>
                  <p class="mb-2 text-base font-thin text-white">Company exhibitions offically starts</p>
                </li>
                <li class="mb-10 ms-4">
                  <div class="absolute w-3 h-3 bg-gray-200 rounded-full mt-1.5 -start-1.5 border border-white"></div>
                  <time class="mb-1 text-sm font-normal leading-none text-white">1.00 PM - 2.00 PM</time>
                  <h3 class="text-lg font-semibold text-orange-500">Platinum Career Talk - NI Malaysia</h3>
                  <p class="mb-4 text-base font-thin text-white">Represented by Selvan Chinappan, the Vice President of APAC Manufacturing and Managing Director of NI Malaysia</p>
                  <span @click.prevent="openModal" class="inline-flex items-center px-4 py-2 text-sm font-medium text-gray-900 bg-white border border-transparent rounded-lg hover:bg-transparent hover:border-white hover:text-white">
                    Register Here
                    <svg class="w-3 h-3 ms-2 rtl:rotate-180" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 10">
                      <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M1 5h12m0 0L9 1m4 4L9 9"/>
                    </svg>
                  </span>
                </li>
                <li class="mb-10 ms-4">
                  <div class="absolute w-3 h-3 bg-gray-200 rounded-full mt-1.5 -start-1.5 border border-white"></div>
                  <time class="mb-1 text-sm font-normal leading-none text-white">2.15 PM - 2.45 PM</time>
                  <h3 class="text-lg font-semibold text-orange-500">Gold Career Talk - Intel Corporation</h3>
                  <p class="mb-4 text-base font-thin text-white">Represented by Wong Mei Fong, General Manager of Penang Assembly Test Manufacturing</p>
                  <a class="disabled inline-flex items-center px-4 py-2 text-sm font-medium text-gray-900 bg-gray-400 border border-transparent rounded-lg">
                    Registered
                  </a>
                </li>
                <li class="mb-10 ms-4">
                  <div class="absolute w-3 h-3 bg-gray-200 rounded-full mt-1.5 -start-1.5 border border-white"></div>
                  <time class="mb-1 text-sm font-normal leading-none text-white">3.00 PM - 3.20 PM</time>
                  <h3 class="text-lg font-semibold text-orange-500">Silver Career Talk - Vitrox Technologies</h3>
                  <p class="mb-4 text-base font-thin text-white">Represented by Yeoh Siew Eng, Manager of People Management</p>
                  <a href="#" @click.prevent="openModal" class="inline-flex items-center px-4 py-2 text-sm font-medium text-gray-900 bg-white border border-transparent rounded-lg hover:bg-transparent hover:border-white hover:text-white">
                    Register Here
                    <svg class="w-3 h-3 ms-2 rtl:rotate-180" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 10">
                      <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M1 5h12m0 0L9 1m4 4L9 9"/>
                    </svg>
                  </a>
                </li>
                <li class="mb-10 ms-4">
                  <div class="absolute w-3 h-3 bg-gray-200 rounded-full mt-1.5 -start-1.5 border border-white"></div>
                  <time class="mb-1 text-sm font-normal leading-none text-white">4.00 PM</time>
                  <h3 class="text-lg font-semibold text-orange-500">Day 1 Ends</h3>
                </li>
              </ol>
            </div>
            
            <!-- Day 2 Timeline -->
            <div>
              <h2 class="text-2xl font-medium text-white mb-2 text-center">Day 2</h2>
              <h3 class="text-xl font-italic text-white dark:text-white mb-6 text-center">3 Jan 2025</h3>
              <ol class="relative border-s border-gray-200">                  
                  <li class="mb-10 ms-4">
                  <div class="absolute w-3 h-3 bg-gray-200 rounded-full mt-1.5 -start-1.5 border border-white"></div>
                  <time class="mb-1 text-sm font-normal leading-none text-white">11.00 AM</time>
                  <h3 class="text-lg font-semibold text-orange-500">Exhibitions</h3>
                  <p class="mb-2 text-base font-thin text-white">Company exhibitions continues for the second day</p>
                </li>
                <li class="mb-10 ms-4">
                  <div class="absolute w-3 h-3 bg-gray-200 rounded-full mt-1.5 -start-1.5 border border-white"></div>
                  <time class="mb-1 text-sm font-normal leading-none text-white">11.30 AM - 12.30 PM</time>
                  <h3 class="text-lg font-semibold text-orange-500">Platinum Career Talk - Fusionex</h3>
                  <p class="mb-2 text-base font-thin text-white">Represented by Lim Yi Yang, General Manager</p>
                  <a class="inline-flex items-center px-4 py-2 text-sm font-medium text-gray-900 bg-gray-400 border border-transparent rounded-lg">
                    Registered
                  </a>
                </li>
                <li class="mb-10 ms-4">
                  <div class="absolute w-3 h-3 bg-gray-200 rounded-full mt-1.5 -start-1.5 border border-white"></div>
                  <time class="mb-1 text-sm font-normal leading-none text-white">1.30 PM - 2.00 PM</time>
                  <h3 class="text-lg font-semibold text-orange-500">Gold Career Talk - Micro Modular System</h3>
                  <p class="mb-4 text-base font-thin text-white">Represented by Joel Nathanael, Operation Director</p>
                  <a href="#" @click.prevent="openModal" class="inline-flex items-center px-4 py-2 text-sm font-medium text-gray-900 bg-white border border-transparent rounded-lg hover:bg-transparent hover:border-white hover:text-white">
                    Register Here
                    <svg class="w-3 h-3 ms-2 rtl:rotate-180" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 10">
                      <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M1 5h12m0 0L9 1m4 4L9 9"/>
                    </svg>
                  </a>
                </li>
                <li class="mb-10 ms-4">
                  <div class="absolute w-3 h-3 bg-gray-200 rounded-full mt-1.5 -start-1.5 border border-white"></div>
                  <time class="mb-1 text-sm font-normal leading-none text-white">2.15 PM - 2.35 PM</time>
                  <h3 class="text-lg font-semibold text-orange-500">Silver Career Talk - Veecotech</h3>
                  <p class="mb-4 text-base font-thin text-white">Represented by Alain Lye, Managing Director</p>
                  <a href="#" @click.prevent="openModal" class="inline-flex items-center px-4 py-2 text-sm font-medium text-gray-900 bg-white border border-transparent rounded-lg hover:bg-transparent hover:border-white hover:text-white">
                    Register Here
                    <svg class="w-3 h-3 ms-2 rtl:rotate-180" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 10">
                      <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M1 5h12m0 0L9 1m4 4L9 9"/>
                    </svg>
                  </a>
                </li>
                <li class="mb-10 ms-4">
                  <div class="absolute w-3 h-3 bg-gray-200 rounded-full mt-1.5 -start-1.5 border border-white"></div>
                  <time class="mb-1 text-sm font-normal leading-none text-white">2.45 PM - 3.00 PM</time>
                  <h3 class="text-lg font-semibold text-orange-500">Bronze Career Talk - Jabil Global Business Center</h3>
                  <p class="mb-4 text-base font-thin text-white">Represented by Rachael Teoh, Project Manager (University Relations & College Recruitment)</p>
                  <a href="#" @click.prevent="openModal" class="inline-flex items-center px-4 py-2 text-sm font-medium text-gray-900 bg-white border border-transparent rounded-lg hover:bg-transparent hover:border-white hover:text-white">
                    Register Here
                    <svg class="w-3 h-3 ms-2 rtl:rotate-180" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 10">
                      <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M1 5h12m0 0L9 1m4 4L9 9"/>
                    </svg>
                  </a>
                </li>
                <li class="mb-10 ms-4">
                  <div class="absolute w-3 h-3 bg-gray-200 rounded-full mt-1.5 -start-1.5 border border-white"></div>
                  <time class="mb-1 text-sm font-normal leading-none text-white">3.30 PM</time>
                  <h3 class="text-lg font-semibold text-orange-500">Closing Ceremony & Lucky Draw</h3>
                </li>      
                <li class="mb-10 ms-4">
                  <div class="absolute w-3 h-3 bg-gray-200 rounded-full mt-1.5 -start-1.5 border border-white"></div>
                  <time class="mb-1 text-sm font-normal leading-none text-white">4.00 PM</time>
                  <h3 class="text-lg font-semibold text-orange-500">CSIRF 2025 Ends </h3>
                </li>
              </ol>
            </div>
          </div>
        </div>

        <!-- Dynamic Modal -->
        <Modal
          v-model:show="isModalVisible"
          :title="modalTitle"
          :text="modalText"
          :color="modalColor"
        >
          <template #actions>
            <button
              @click.prevent="isModalVisible = false"
              class="px-4 py-2 bg-gray-400 text-white rounded-lg hover:bg-gray-600"
            >
              Cancel
            </button>
            <button
              @click.prevent="user != null ? registerEvent() : $router.push('/login')"
              class="px-4 py-2 bg-orange-500 text-white rounded-lg hover:bg-orange-600"
            >
              {{user != null ? 'Confirm' : 'Log In'}}
            </button>
          </template>
        </Modal>
    </section>
</template>

<script>
import Modal from "@/components/Modal.vue";
import { auth, db } from '@/firebase';
import { onAuthStateChanged } from 'firebase/auth';
import { doc, getDoc } from 'firebase/firestore';

export default {
  components: {
    Modal,
  },
  data() {
    return {
      user: null,
      isModalVisible: false,
      modalTitle: "",
      modalText: "",
      modalColor: "white",
    };
  },
  methods: {
    openModal() {
      if (this.user != null) {
        this.modalTitle = "RSVP Now";
        this.modalText = "Confirm to join the talk?";
        this.modalColor = "black";
        this.isModalVisible = true;
      } 
      else {
        this.modalTitle = "Login Required";
        this.modalText = "Please log in to register for the event";
        this.modalColor = "black";
        this.isModalVisible = true;
      }
    },
    registerEvent() {
      toastr.success('You have registered the event!', 'Success');
      this.isModalVisible = false;
    },
  },

  async created() {
    onAuthStateChanged(auth, async (currentUser) => {
      if (currentUser) {
        const userDoc = await getDoc(doc(db, 'users', currentUser.uid));
        if (userDoc.exists()) {
          this.user = userDoc.data();
        }
      }
    });
  },
};
</script>