<script setup lang="ts">
import { ref, computed } from 'vue'

type Selection = 'approve' | 'disapprove' | 'no-opinion' | null

const selected = ref<Selection>(null)
const isLoading = ref(false)
const voted = ref<Selection>(null)

const voteMessage = computed(() => {
  if (voted.value === 'approve') return 'ท่านลงมติเห็นชอบ'
  if (voted.value === 'disapprove') return 'ท่านลงมติไม่เห็นชอบ'
  if (voted.value === 'no-opinion') return 'ท่านลงมติไม่แสดงความเห็น'
  return ''
})

const catImage = computed(() => {
  if (voted.value === 'approve') return '/cat_with_flower.webp'
  if (voted.value === 'disapprove') return '/cat_with_knife.webp'
  if (voted.value === 'no-opinion') return '/cat_with_book.webp'
  return ''
})

const voteCaption = computed(() => {
  if (voted.value === 'approve') return 'จัดไปฮาฟฟู่ว สุดสวย 💖'
  if (voted.value === 'disapprove') return 'ระวังตัวไว้งับ 😤'
  if (voted.value === 'no-opinion') return '...😐'
  return ''
})

function selectBox(value: Selection) {
  selected.value = value
  isLoading.value = true
  voted.value = value

  setTimeout(() => {
    isLoading.value = false
  }, 2000)
}

function resetVote() {
  selected.value = null
  voted.value = null
}
</script>

<template>
<div class="bg-gray-100 flex items-center justify-center min-h-screen p-4 sm:p-10">

    <!-- Loading Modal -->
    <div v-if="isLoading" class="fixed inset-0 bg-black/50 flex items-center justify-center z-50">
      <div class="bg-white border-2 border-black p-8 sm:p-12 text-center">
        <div class="animate-spin w-16 h-16 sm:w-20 sm:h-20 border-4 border-black border-t-transparent rounded-full mx-auto mb-6"></div>
        <p class="text-xl sm:text-2xl font-bold">{{ voteMessage }}</p>
        <p class="text-sm sm:text-base mt-2">กำลังบันทึก...</p>
      </div>
    </div>

    <!-- Result Page -->
    <div v-if="voted && !isLoading" class="w-full max-w-[700px] bg-white border-2 border-black shadow-2xl p-8 sm:p-12 text-center">
      <img :src="catImage" alt="cat" class="w-40 h-40 sm:w-52 sm:h-52 object-cover mx-auto mb-6 rounded-full border-2 border-black">
      <div class="text-6xl sm:text-8xl mb-6">✓</div>
      <h1 class="text-2xl sm:text-3xl font-bold mb-4">{{ voteMessage }}</h1>
      <p class="text-lg mb-8">{{ voteCaption }}</p>
      <button
        @click="resetVote"
        class="bg-[#f7e171] border-2 border-black px-6 py-3 font-bold hover:bg-yellow-400 cursor-pointer"
      >
        ลงมติใหม่
      </button>
    </div>

    <!-- Voting Ballot -->
    <div v-if="!voted" class="w-full max-w-[700px] bg-white border-2 border-black shadow-2xl relative overflow-hidden">

        <!-- Main Ballot Section (Yellow) -->
        <div class="bg-[#f7e171] p-8 sm:p-12 text-center border-b-2 border-black relative">

            <div class="relative z-10">
                <h1 class="text-2xl sm:text-3xl font-bold mb-4 tracking-tight">บัตรออกเสียงหวานใจมติ</h1>
                <h2 class="text-lg sm:text-xl font-bold mb-6">ประเด็น
            "ท่านเห็นชอบว่าสมควรมีการเดทวันวาเลนไทน์ย้อนหลังหรือไม่"</h2>
                <p class="text-sm sm:text-base mb-10 leading-snug">
                    โปรดทำเครื่องหมายกากบาท X<br>
                    ในช่องทำเครื่องหมายเพียงเครื่องหมายเดียว
                </p>

                <!-- Voting Boxes Area -->
                <div class="flex flex-row justify-around items-center gap-2">
                    <div class="w-28 sm:w-40">
                        <div class="bg-white border border-black p-2 font-bold text-base sm:text-lg mb-2 shadow-sm">เห็นชอบ</div>
                        <div
                            class="bg-white border-2 border-black h-20 sm:h-24 flex items-center justify-center cursor-pointer hover:bg-gray-50"
                            @click="selectBox('approve')"
                        >
                            <span v-if="selected === 'approve'" class="text-4xl sm:text-5xl font-bold text-black">✕</span>
                        </div>
                    </div>

                    <div class="flex flex-col sm:flex-row items-center font-bold text-xs sm:text-sm">
                        <span class="text-xl sm:text-3xl mx-1">⬅</span>
                        <span class="whitespace-nowrap">ช่องทำเครื่องหมาย</span>
                        <span class="text-xl sm:text-3xl mx-1">➡</span>
                    </div>

                    <div class="w-28 sm:w-40">
                        <div class="bg-white border border-black p-2 font-bold text-base sm:text-lg mb-2 shadow-sm">ไม่เห็นชอบ</div>
                        <div
                            class="bg-white border-2 border-black h-20 sm:h-24 flex items-center justify-center cursor-pointer hover:bg-gray-50"
                            @click="selectBox('disapprove')"
                        >
                            <span v-if="selected === 'disapprove'" class="text-4xl sm:text-5xl font-bold text-black">✕</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Footer (No Opinion) Section -->
        <div class="flex flex-row h-24 sm:h-32">
            <div class="flex-grow p-4 sm:p-6 flex items-center text-sm sm:text-lg font-bold bg-white">
                ถ้าไม่ประสงค์จะแสดงความคิดเห็นให้ทำเครื่องหมายกากบาท X ในช่อง
                <span class="ml-auto text-2xl sm:text-4xl">➡</span>
            </div>
            <div class="w-28 sm:w-40 border-l-2 border-black flex flex-col bg-white cursor-pointer hover:bg-gray-50" @click="selectBox('no-opinion')">
                <div class="text-[10px] sm:text-xs text-center p-1 border-b border-black font-bold leading-tight">
                    ไม่แสดง<br>ความคิดเห็น
                </div>
                <div class="flex-grow flex items-center justify-center">
                    <span v-if="selected === 'no-opinion'" class="text-4xl sm:text-5xl font-bold text-black">✕</span>
                </div>
            </div>
        </div>
    </div>
</div>
</template>

<style scoped></style>
