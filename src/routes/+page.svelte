<script lang="ts">
    let pertanyaan: string[] = [
      "Apakah Anda sering merasakan nyeri atau tekanan di dada saat beraktivitas maupun saat istirahat?",
      "Apakah Anda merasa cepat lelah bahkan saat melakukan aktivitas ringan seperti berjalan atau menaiki tangga?",
      "Apakah Anda pernah mengalami sesak napas secara tiba-tiba tanpa sebab yang jelas?",
      "Apakah Anda sering berkeringat dingin secara tiba-tiba, terutama saat sedang tidak beraktivitas berat?",
      "Apakah Anda pernah merasakan detak jantung yang sangat cepat, tidak teratur, atau seperti berdebar-debar?",
      "Apakah Anda pernah mengalami pusing berat, hampir pingsan, atau benar-benar pingsan tanpa sebab jelas?",
      "Apakah Anda memiliki riwayat tekanan darah tinggi yang tidak terkontrol?",
      "Apakah ada anggota keluarga Anda yang memiliki riwayat penyakit jantung atau serangan jantung?"
    ];
  
    let index: number = 0;
    let skor: number = 0;
    let selesai: boolean = false;
    let hasil: string = "";
    let loading: boolean = false;
    let jawaban: boolean | null = null;
    let semuaJawaban: { soal: string; jawaban: string }[] = [];
  
    function lanjut(): void {
      if (jawaban === null) return;
      semuaJawaban.push({
        soal: pertanyaan[index],
        jawaban: jawaban ? "Iya" : "Tidak"
      });
  
      if (jawaban) skor++;
      jawaban = null;
      index++;
  
      if (index >= pertanyaan.length) {
        selesai = true;
        loading = true;
        setTimeout(() => {
          loading = false;
          if (skor >= 6) {
            hasil = `<p>Tingkat risiko <span class="text-red-600 font-bold">tinggi</span>. Segera konsultasikan ke dokter. </p>`;
          } else if (skor >= 3) {
            hasil = `<p>Tingkat risiko <span class="text-yellow-600 font-bold">sedang</span>. Perlu pemeriksaan lebih lanjut. </p>`;
          } else {
            hasil = `<p>Tingkat risiko <span class="text-green-600 font-bold">rendah</span>. Tetap jaga kesehatan dan pola hidup sehat. </p>`;
          }
        }, 1000);
      }
    }
  </script>
  
  <div class="max-w-2xl mx-auto mt-20 p-6 border-4 border-blue-500 rounded-xl shadow-md bg-pink-300">
    <h1 class="text-2xl font-bold text-center text-black mb-4">
      "Sistem Pendeteksi Gejala Penyakit Jantung"
    </h1>
  
    {#if !selesai}
      <div class="border-4 border-blue-500 rounded-xl p-6 my-8 text-center shadow-md bg-white">
        <p class="text-lg  font-medium mb-4">{pertanyaan[index]}</p>
  
        <div class="flex justify-center gap-8">
          <label class="flex items-center gap-2 cursor-pointer">
            <input type="radio" bind:group={jawaban} value={true} class="radio radio-success" />
            <span class="text-lg">Iya</span>
          </label>
          <label class="flex items-center gap-2 cursor-pointer">
            <input type="radio" bind:group={jawaban} value={false} class="radio radio-error" />
            <span class="text-lg">Tidak</span>
          </label>
        </div>
  
        <button on:click={lanjut} class="btn btn-primary mt-6">Lanjut</button>
      </div>
    {:else}
      <div class="text-center py-4">
        <h2 class="text-xl font-semibold text-gray-800 mb-4">Jawaban Anda:</h2>
        <ul class="text-left space-y-3">
          {#each semuaJawaban as item, i}
            <li class="border border-gray-300 p-3 rounded-lg">
              <strong>Pertanyaan {i + 1}:</strong> {item.soal} <br />
              <span class="font-semibold">Jawaban:</span> {item.jawaban}
            </li>
          {/each}
        </ul>
  
        {#if loading}
          <div class="mt-8 text-lg text-blue-500 font-semibold animate-pulse">
            Menganalisis hasil...
          </div>
        {:else}
          <div class="mt-8 text-xl font-bold text-blue-500">{@html hasil}</div>
          <p class="mt-4 text-sm text-yellow-600 font-semibold italic">
            ⚠️ Warning: Jangan percaya 100%!
          </p>
        {/if}
      </div>
    {/if}
  </div>