<template>
  <div class="layout">
    <!-- Sidebar Region -->
    <aside class="sidebar">
      <h2>BOSS IN TEYVAT</h2>
      <ul>
        <li
          v-for="r in menu"
          :key="r.name"
          :class="{ active: selectedRegion === r.name }"
          @click="selectedRegion = r.name"
        >
          {{ r.emoji }} {{ r.name === 'Target' ? 'Target Boss' : r.name }}
        </li>
      </ul>
    </aside>

    <!-- Main Content -->
    <main class="main-content">
      <h1 v-if="selectedRegion === 'Target'">⭐ Target Boss</h1>
      <h1 v-else>Boss In {{ selectedRegion }}</h1>

      <div class="monster-grid">
        <div
          v-for="boss in displayedBosses"
          :key="boss.id"
          class="monster-card"
        >
          <img :src="boss.img" :alt="boss.name" />
          <h3>{{ boss.name }}</h3>
          <p>{{ boss.desc }}</p>
          <button @click="toggleTarget(boss)">
            {{ isTarget(boss) ? '⭐ Target' : '☆ Jadikan Target' }}
          </button>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedRegion: 'Mondstadt',
      menu: [
        { name: 'Mondstadt', emoji: '🌬️' },
        { name: 'Liyue', emoji: '🪨' },
        { name: 'Inazuma', emoji: '⚡' },
        { name: 'Sumeru', emoji: '🌿' },
        { name: 'Fontaine', emoji: '💧' },
        { name: 'Natlan', emoji: '🔥' },
        { name: 'Target', emoji: '⭐' }
      ],
      targets: [],
      bosses: [
        // MONDSTADT
        { id: 1, region: 'Mondstadt', name: 'Stormterror Dvalin', desc: 'Naga penjaga lama Mondstadt yang kehilangan kendali.', img: 'https://ih1.redbubble.net/image.5299523060.2908/bg,f8f8f8-flat,750x,075,f-pad,750x1000,f8f8f8.u1.jpg' },
        { id: 2, region: 'Mondstadt', name: 'Anemo Hypostasis', desc: 'Manifestasi murni dari elemen Anemo.', img: 'https://bbs.hoyolab.com/hoyowiki/picture/enemy/Anemo%20Hypostasis_icon.png' },
        { id: 30, region: 'Mondstadt', name: 'Cryo Regisvine', desc: 'Tanaman beku yang agresif di Dragonspine.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTHnjZCipN3mxSIdVGLuvrYiP-Rr4mVb9223g&s' },
        { id: 31, region: 'Mondstadt', name: 'Andrius (Wolf of the North)', desc: 'Serigala penjaga Mondstadt dari masa lampau.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTH86RVNIp7R8SzR46J9ooZofF2cfSFqsE-Gw&s' },
        { id: 32, region: 'Mondstadt', name: 'Frostarm Lawachurl', desc: 'Musuh elite dengan kekuatan Cryo tinggi.', img: 'https://i.pinimg.com/736x/75/a2/77/75a27757ef920f0f6a8c8f0addf216d7.jpg' },

        // LIYUE
        { id: 3, region: 'Liyue', name: 'Geo Hypostasis', desc: 'Makhluk kristal Geo yang kuat.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR52FYOplr4pBcEhLLbyPVZHWY8tK0eFN1pug&s' },
        { id: 4, region: 'Liyue', name: 'Azhdaha', desc: 'Naga purba yang bangkit dari dalam bumi Liyue.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT2Pz5MuNhbZqxzmICeGreA24jGTt77I8Ehfg&s' },
        { id: 33, region: 'Liyue', name: 'Primo Geovishap', desc: 'Makhluk purba dengan serangan elemen kuat.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRFOu8Q5dBjojf_jETG5u3Q0nUUdTb2lYym3g&s' },
        { id: 34, region: 'Liyue', name: 'Oceanid', desc: 'Makhluk air yang memanggil ilusi.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQGaqhLysFWb9m5AhVfqsiTQLyPWyNjZpVq5A&s' },
        { id: 35, region: 'Liyue', name: 'Ruin Serpent', desc: 'Mesin purba dari tambang Chasm.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSiRzQ4d7RSEd1IeyzToNOzdaH-1MwopPn8gA&s' },

        // INAZUMA
        { id: 5, region: 'Inazuma', name: 'Raiden Shogun Puppet', desc: 'Boneka pelindung abadi yang kuat dan cepat.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTFk-k0fe0aZyKKoOFHdk0ca3L2cKi8toZR_1-5rqnuCULsC34x6W5ZHhRQuNJvZE69_8E&usqp=CAU' },
        { id: 6, region: 'Inazuma', name: 'Thunder Manifestation', desc: 'Oceanid elemen elektro.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcShdbm4QhGiu2VypTZIVuruiTyu6k291sDWKg&s' },
        { id: 36, region: 'Inazuma', name: 'Hydro Hypostasis', desc: 'Makhluk elemen air berbentuk kubus.', img: 'https://bbs.hoyolab.com/hoyowiki/picture/enemy/Hydro%20Hypostasis_icon.png' },
        { id: 37, region: 'Inazuma', name: 'Maguu Kenki', desc: 'Samurai mekanis dengan kekuatan Anemo dan Cryo.', img: 'https://lagazettedeteyvat.fr/wp-content/uploads/2024/11/Lame-oni.webp' },
        { id: 38, region: 'Inazuma', name: 'Pyro Hypostasis', desc: 'Konstruk Pyro yang melindungi dirinya dengan kristal.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTAKAzQ1F7gVYDXTM7D87iQ580qUDJm-B7JHf21Ip42k3BdVM8YDblxVkBHT34U-GZNwJk&usqp=CAU' },

        // SUMERU
        { id: 7, region: 'Sumeru', name: 'Scaramouche (The Shouki no Kami)', desc: 'Boss weekly berbasis elektro.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRXCYWe_oDcUPplC2lY47bdDTa6cOuutTtBWQ&s' },
        { id: 8, region: 'Sumeru', name: 'Dendro Regisvine', desc: 'Tanaman buas dengan kekuatan Dendro.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRDksnmeNJxTE1nCkJ3oHg38WszQ4kKaAbJGI4TT3ALJKyBO7kOnlutvc5OPiOrkG6g-S4&usqp=CAU' },
        { id: 39, region: 'Sumeru', name: 'Algorithm of Semi-Intransient Matrix of Overseer Network', desc: 'Mesin canggih pengendali pasir di padang gurun.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQyguk1mn7sbKNbZHDC-G5N7dY_kGxrOAXUFA&s' },
        { id: 40, region: 'Sumeru', name: 'Setekh Wenut', desc: 'Cacing gurun raksasa dari bawah tanah.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTatqmMjvVL5MId1kbSNxMqRqH6cOAbhZvqgQ&s' },
        { id: 41, region: 'Sumeru', name: 'Consecrated Beast', desc: 'Binatang liar terkutuk dengan kekuatan tinggi.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSLDgGL0_uBYVfcEIXxt3Q5bR4Fd8PzgtIcIg&s' },

        // FONTAINE
        { id: 9, region: 'Fontaine', name: 'Icewind Suite', desc: 'Pasangan mekanis dengan serangan simfoni.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQBIfioGrd8TthmCLdvFVsvbQAA0vsDVjJXSQhInG3_82Lqm9AMl_kL0YX5NvR4zjObRDY&usqp=CAU' },
        { id: 10, region: 'Fontaine', name: 'Experimental Field Generator', desc: 'Mesin besar yang mengamuk di perairan Fontaine.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQpO8z_9wYwCz9k18ecUxy-vRAhFhGtEw42pg&s' },
        { id: 42, region: 'Fontaine', name: 'Emperor of Fire and Iron', desc: 'Boss dengan kekuatan Pyro dan mesin pertunjukan.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcThgOWNqaG2EMoMuNloAjPX98KRWNUalrsm8A&s' },
        { id: 43, region: 'Fontaine', name: 'Armored Crab', desc: 'Crab kuat dengan armor dan pukulan berat.', img: 'https://gensh.honeyhunterworld.com/img/m_61621.webp' },

        // NATLAN
        { id: 11, region: 'Natlan', name: 'Xiuhcoatl', desc: 'Ular api mitologis Aztek yang menjadi senjata dewa matahari Huitzilopochtli.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSyKUEmqXQfTeonCqophZBMRMl5841snxwEzA&s' },
        { id: 44, region: 'Natlan', name: 'Gluttonous Yumkasaur Mountain King', desc: 'Raja gunung raksasa bertubuh dinosaurus yang rakus dan menakutkan.', img: 'https://act-upload.hoyoverse.com/event-ugc-hoyowiki/2025/04/16/35428890/89bd4c368077401177fcfc5efdb0458d_6218384140036125671.png?x-oss-process=image%2Fformat%2Cwebp' },
        { id: 45, region: 'Natlan', name: 'Goldflame Qucusaur Tyrant', desc: 'Naga mirip dinosaurus dengan api emas yang berkuasa sebagai tiran ganas..', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTeFzLZdoFzWPoJtXjUiG55XiRenoCrL_mmJs1krEoLGzVqWzodOEWzdDTUCx33E2r1x9w&usqp=CAU' },
      ]
    }
  },
  computed: {
    displayedBosses() {
      return this.selectedRegion === 'Target'
        ? this.targets
        : this.bosses.filter(b => b.region === this.selectedRegion)
    }
  },
  methods: {
    toggleTarget(boss) {
      const index = this.targets.findIndex(t => t.id === boss.id)
      if (index !== -1) {
        this.targets.splice(index, 1)
      } else {
        this.targets.push(boss)
      }
    },
    isTarget(boss) {
      return this.targets.some(t => t.id === boss.id)
    }
  }
}
</script>

<style scoped>
.layout {
  display: flex;
  min-height: 100vh;
  font-family: 'Segoe UI', sans-serif;
}

.sidebar {
  width: 240px;
  background: #f8f4ea;
  color: #333;
  padding: 20px;
  border-right: 1px solid #ddd;
}

.sidebar h2 {
  font-size: 1.4em;
  margin-bottom: 20px;
  color: #5a4d3b;
}

.sidebar ul {
  list-style: none;
  padding: 0;
}

.sidebar li {
  padding: 10px 12px;
  cursor: pointer;
  border-radius: 8px;
  margin-bottom: 10px;
  background-color: #f3e9d2;
  transition: background 0.2s;
}

.sidebar li.active,
.sidebar li:hover {
  background-color: #e2d4b7;
  font-weight: bold;
}

.main-content {
  flex: 1;
  background: #f9f9fb;
  padding: 20px;
}

.monster-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
}

.monster-card {
  background: #fff;
  border-radius: 12px;
  padding: 15px;
  text-align: center;
  box-shadow: 0 2px 5px rgba(0,0,0,0.08);
}

.monster-card img {
  width: 200px;
  height: 200px;
}

.monster-card h3 {
  margin: 10px 0 5px;
}

.monster-card p {
  font-size: 0.85em;
  color: #555;
}

.monster-card button {
  margin-top: 10px;
  background: #d49a6a;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
}

.monster-card button:hover {
  background: #bb8657;
}

@media (max-width: 768px) {
  .monster-grid {
    grid-template-columns: 1fr;
  }
}
</style>
